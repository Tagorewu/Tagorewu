---
title: '基于Altera FPGA的 摄像头+LCD DIY'
date: Sat, 30 Nov 2013 09:54:20 +0000
draft: false
categories:
  - "FPGA"
tags: ['FPGA']
---

        前些日子买了个大家玩得很火的OV7670来玩玩，由于之前没玩过摄像头，也听说不带FIFO的摄像头采集起来是那么困难，所以就买了个不带FIFO的来尝尝鲜。后来看了资料才知道，一幅QVGA的图像按320\*240\*2Byte来算，也要153.6KB内存，对单片机来说是一比不小的开销。更何况单片机的速率才多少M，就按50M主频来说，捕捉一个8M左右的像素时钟也是相当困难的。当然后面也用STM32F103ZET6来尝试捕捉了一下，结果也是不尽人意。 考虑到时钟的捕捉，当然是用FPGA的硬件电路来实现才是王道了。又想想摄像头和液晶屏的初始化也是相当麻烦的，如果也用硬件电路来实现，那写起代码来就是一大串了，而且改起来也不方便。于是乎，脑子里一直存在的一个想法在这里就能得到真正的应用和实现了。那就是MCU与FPGA结合的方案。手上正有一款Altera 的EP2C8Q208C的开发板。那么我们的DIY就开始了。

[![QQ图片20131130163312](http://www.wuquantai.com/wp-content/uploads/2013/11/QQ图片20131130163312-300x220.jpg)](http://www.wuquantai.com/wp-content/uploads/2013/11/QQ图片20131130163312.jpg) 

        首先要准备的东西就是 一个OV7670摄像头，一块TFT液晶屏，一块FPGA核心板。使用Nios II做为主控MCU会使你的DIY简洁很多。系统主要的结构就如下：

[![bdf](http://www.wuquantai.com/wp-content/uploads/2013/11/bdf-300x168.jpg)](http://www.wuquantai.com/wp-content/uploads/2013/11/bdf.jpg) 

        一个Nios II MCU，一个VGA转8080并口的模块VGA\_8080，一个MCU与VGA\_8080模块分别对LCD控制的总线切换模块。模块分工，MCU负责初始化液晶屏和OV7670。 VGA\_8080负责OV7670的VGA输出时序转换成LCD的16bit的8080并口时序 。由于我的LCD是320\*240的，所以我们只 使用QVGA输出，并且LCD使用的是565的像素格式，我们也将OV7670配置成565像素格式，这样能够使我们的编程方便很多。 我们的主要任务分为以下几块：1.nios ii MCU的搭建; 2.nios ii软件程序的编写；3.fpga硬件代码的编写。 一、NIOS ii MCU搭建可参考《NIOS ii 那些事儿》，硬件上要求板子上带有SDRAM, 否则内存不够会很麻烦。等系统整体编译通过后可以把代码写入flash当中，这样每次掉电后开机就能直接运行了。主要配置一个LCD的控制口，SDRAM的控制口，OV7670的SCCB控制口。这里有几个注意点：1.FPGA的双向口处理，由于SCCB的SIOD数据口是双向的，所以在配置时要配置成双向口，画顶层Pin时也要用bidir。2.本来液晶屏的数据口也是双向的，但是在FPGA内部没有三态门，按顶层那样的连接无法实现双向，要实现的话只能通过将IO口引出来，再把IO口连接到其它pin脚输入，不过这样很麻烦，就省了，LCD代码中要改动LCD读数据的部分。（不知道各位还有什么好办法能实现内部的双向口？） 二、然后就是LCD代码，和OV7670初始化代码的移植，之前都是在STM32上跑的，想办法把它们移植到NIOS ii当中，基本上只需修改IO口读写部分的代码就行了。 三、关键部分就是FPGA硬件上实现 OV7670的QVGA时序到液晶屏读写时序的转换。Bus\_Switcher就是个2选一的多路选择器，负责切换LCD的控制总线。在主MCU初始化液晶屏和OV7670之后，就把液晶屏的控制总线交给VGA\_8080控制模块，直接将OV7670的QVGA时序写入到液晶屏的GRAM当中，这样显示的帧频就能达到很高，画面非常流畅。在后期设计中可以再设置一个存储器，将图片保存下来。这里仅仅实现的摄像头信号的高速显示。这里的代码基本上可达到要求了，还有些小细节要优化，后期还可以添加一系列按钮调整OV7670的各种参数，非常之方便。

  

> ```
> ```
> module VGA\_8080(iclk, rst\_n, XCLK, HREF, VSYNC, PCLK, C\_DATA, LCD\_CS, LCD\_WR, LCD\_RD, LCD\_RS, LCD\_RST, LCD\_DATA);
> input rst\_n;
> input iclk;
> //   camera IO;
> output XCLK;
> input HREF, VSYNC, PCLK;
> input \[7:0\] C\_DATA;
> //LCD IO;
> output LCD\_WR, LCD\_RD, LCD\_RS, LCD\_RST, LCD\_CS;
> output \[15:0\] LCD\_DATA;
> 
> //-------------------------------XCLK -----------------------------//
> reg \[3:0\] div\_cnt;
> reg r\_XCLK;
> assign XCLK = r\_XCLK;
> 
> always @ (posedge iclk)
> begin
> 	if(div\_cnt < 4'b0010)	//50/2/(2+1)=8.33M
> 		div\_cnt <= div\_cnt + 1'b1;
> 	else begin
> 		r\_XCLK <= ~r\_XCLK;
> 		div\_cnt <= 4'b0;
> 	end
> end
> 
> //---------------write to LCD--------------------------------//
> 
> reg r\_WR, r\_RD, r\_RS, r\_CS;
> reg \[3:0\] wr\_step;
> reg \[3:0\] disp\_state;
> reg \[3:0\] reset\_step;
> reg wr\_reg;       //flag show whether need to write reg
> reg \[15:0\] LCD\_Reg;
> reg \[15:0\] LCD\_RegValue;
> assign LCD\_WR = r\_WR;
> assign LCD\_RD = r\_RD;
> assign LCD\_RS = r\_RS;
> assign LCD\_CS = r\_CS;
> assign LCD\_RST = 1'b1;
> reg F\_LSB;      //flag show LSB or MSB
> reg \[8:0\] MSB;
> reg \[15:0\] r\_LCD\_DATA;
> 
> assign LCD\_DATA = r\_LCD\_DATA;
> 
> always @ (posedge PCLK or negedge rst\_n)
> begin
> 	if(!rst\_n)
> 	begin
> 		wr\_step <=	4'b0;     //write reg time series
> 		disp\_state <=	4'b0; //write pixel to GRAM
> 		reset\_step <= 4'b0;   //reset cursor to 0,0 
> 		r\_RS <= 1;
> 		r\_CS <= 1;
> 		r\_WR <= 1;
> 		r\_RD <= 1;
> 		F\_LSB <= 0;
> 		MSB <= 0;
> 		r\_LCD\_DATA <= 0;
> 	end	
> 	else 
> 	if(!wr\_reg)
> 	begin
> 		case(disp\_state)
> 				4'd0 : 	//state to reset LCD cursur
> 						case(reset\_step)
> 								4'd0 : begin 
> 									    r\_CS <= 1'b1;
> 										LCD\_Reg <= 16'h0020 ; // Y pos reg
> 										LCD\_RegValue <= 16'd0; //
> 									    wr\_reg <= 1; //start to write reg
> 									    reset\_step <= reset\_step + 4'd1;
> 									    end
> 								4'd1 : begin
> 									    r\_CS <= 1'b1;
> 										LCD\_Reg <= 16'h0021; // X pos reg
> 										LCD\_RegValue <= 16'd319;
> 									    wr\_reg <= 1; //start to write reg
> 									    reset\_step <= reset\_step + 4'd1;
> 									    end	
> 								4'd2 : begin
> 										r\_LCD\_DATA <= 16'h0022;	//write GRAM prepare
> 										r\_RS <= 0; //write reg
> 										r\_CS <= 0;
> 										r\_WR <= 0;
> 										r\_RD <= 1;
> 										reset\_step <= reset\_step + 4'd1;
> 										end
> 								4'd3 : begin
> 										r\_WR <= 1;
> 										reset\_step <= reset\_step + 4'd1;
> 										end
> 								4'd4 : begin 
> 										r\_CS <= 1;
> 										r\_RS <= 1;	//								
> 										if(!VSYNC) //wait VSYNC over
> 										begin
> 											disp\_state <= 4'd1;  //start new frame
> 											reset\_step <= 4'd0;	 //reset for next
> 											r\_CS <= 0;
> 										end
> 									   end
> 								default : begin
> 										disp\_state <= 4'd0;
> 										wr\_reg <=0;
> 										reset\_step <= 4'd0;
> 										end
> 						endcase 		
> 				4'd1 :	//wait to display
> 						if(VSYNC) 
> 							disp\_state <= 4'd0; // VSYNC need to reset cursor
> 						else 			
> 							if(HREF)
> 							begin
> 								case(F\_LSB)
> 									1'b0 :	begin
> 											MSB <= C\_DATA;
> 											F\_LSB <= 1'b1;
> 											r\_WR <= 1'b1;
> 											end
> 									1'b1 :	begin
> 											r\_LCD\_DATA <= {MSB, C\_DATA};
> 											F\_LSB <= 1'b0;
> 											r\_WR <= 1'b0;
> 											end
> 									default : F\_LSB <= 0;
> 								endcase 
> 							end	
> 
> 				default : disp\_state <= 4'b0;
> 		endcase 
> 	end
> 	else // write(LCD\_Reg,LCD\_RegValue);
> 	case(wr\_step)
> 		4'd0 :	begin	
> 				r\_LCD\_DATA <= LCD\_Reg;	//write reg
> 				r\_RS <= 0;
> 				r\_CS <= 0;
> 				r\_WR <= 0;
> 				r\_RD <= 1;
> 				wr\_step <= wr\_step + 4'd1;
> 				end
> 		4'd1 : begin
> 				r\_WR <= 1;
> 				wr\_step <= wr\_step + 4'd1;									
> 			   end	
> 		4'd2 : begin
> 			   r\_LCD\_DATA <= LCD\_RegValue; //write value
> 			   r\_RS <= 1;
> 			   r\_WR <= 0;
> 			   wr\_step <= wr\_step + 4'd1;
> 			   end
> 		4'd3 : begin
> 				r\_WR <= 1;
> 				wr\_step <= wr\_step + 4'd1;
> 			   end
> 		4'd4 : begin
> 			    r\_CS <= 1;
> 			    wr\_step <= 0;
> 			    wr\_reg <= 0; //write reg over
> 			   end
> 		default : begin
> 					wr\_step <= 0;
> 					wr\_reg <= 0;
> 				end			
> 	endcase
> end 
> endmodule
> ```
> 
> ```

  这里代码有一个优点就在帧同步信号到来之后将液晶屏的游标指针初始化到（0，0）的位置重新开始写，这样就可以做到一个真正的帧同步。 之前参考了许多代码都只是通过计像素个数到76800之后完成 一帧的显示。 完成之后就能达到如下的效果了。外部连线非常少，只有几根电源线。充分体现了FPGA的厉害之处。由于选的是NIOS II/f 功能最全的软核，整个系统编译下来也只用了大约3300个逻辑单元。

  [![QQ图片20131130163243](http://www.wuquantai.com/wp-content/uploads/2013/11/QQ图片20131130163243-300x154.jpg)](http://www.wuquantai.com/wp-content/uploads/2013/11/QQ图片20131130163243.jpg)   

未完待续...