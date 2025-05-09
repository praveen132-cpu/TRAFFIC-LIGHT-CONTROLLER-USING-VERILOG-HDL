# Traffic Light Controller Using Verilog
* The purpose of this project is to design a methodology using Verilog to control the traffic with specified time delays for a T-Shaped road.*

# Table of Contents

1. [Introduction](#Introduction)
2. [Methodology](#Methodology)
    - [Directions Considered](#directions-considered)
    - [Problem Statement](#problem-statement)
    - [State Diagram](#state-diagram)
    - [State Table](#state-table)
3. [RTL Code](#rtl-code)
    - [RTL Schematic View](#rtl-schematic-view)
4. [TESTBENCH](#testbench)
5. [Output Waveforms](#output-waveforms)
6. [Result](#result)
7. [Future work](#future-work)
8. [References](#references)
9. [Author](#author)


## Introduction 

The main purpose of the traffic light control system is to control the congestion of vehicles at the junctions and also for safer pedestrian crossing. Traffic control in cities is challenging due to the high number of vehicles and the dynamic nature of traffic systems. Inefficient traffic management leads to accidents and time losses. Traditional traffic light control (TLC) systems rely on microcontrollers or microprocessors, operating on fixed- time intervals without real-time flexibility. To address these limitations, an FPGA- based traffic control system is proposed. This Project proposes the reconfigurable Traffic Light controller which can display the time of waiting in all the directions. It also prioritizes emergency vehicles by sensing their presence and providing right-of-way on the specific road. The FPGA-based design allows real-time modifications to the program, offering more adaptability compared to traditional systems. The hardware implementation is programmed using Verilog HDL and tested on Xilinx Vivado 2024.2 software. The main objective of this project is design, implementation, synthesis, simulation, evaluation of area, power delay of advanced traffic light control system. This system significantly improves traffic flow management, reduces delays, and enhances safety by responding effectively to varying traffic conditions and emergencies
Software Required: Xilinx Vivado 2024.2


Language Used: Verilog HDL


Keywords: Traffic Light Controller, Verilog HDL, FSM, FPGA, Traffic Management.

#### The Verilog language has been selected for programming the FPGA to fill two important needs in the design process. 

- Firstly, it gives full description of the structure of a design that is how it is decomposed into sub-designs, and how those sub-designs are interconnected. 
- Secondly, it allows simulating the design before starting the manufacturing. 
- Accordingly, the designers can quickly compare alternatives and test for correctness without the delay and expense of hardware prototyping. 

#### Benefits of Using Verilog HDL (Hardware Description Language) : 

Verilog is a widely used Hardware Description Language (HDL) for designing digital circuits. It can also be used for modeling analog circuits. Verilog is a descriptive language that describes a relationship between signals in a circuit. 

A Verilog model describes a unit of digital hardware in terms of :
- Interconnections of other hardware unit whose models prescribe their behavior in a 
    simulation.
- Behavioral / procedural algorithms that abstractly describe input/output behavior    
       that could be personified in a hardware unit. 

Hardware description language (HDL) is divided by two types, Verilog and VHDL (VHSIC – Very High Speed Integrated Circuit Hardware Description Language). Both have its advantages and its disadvantage. 
In this project, Verilog HDL was chosen because it’s used for synthesis of logic circuits (synthesizable code), used for verification purposes of a circuit (can be analog or digital or mixed signal), can be used by combining synthesis & verification (synthesizable & behavioral code) and it used for netlist representation of a synthesizable circuit (structural code). 

#### The advantages using Verilog HDL are shown below : 
- Easy to write. 
- Easy to understand as it similar to C program. 
- Easier to learn compared with VHDL.


## Methodology

### Directions Considered

![directions](https://user-images.githubusercontent.com/83152452/131366734-67c76e3c-b53d-49ca-a5ba-fb058d19d578.png)

The directions, M1, MT, M2, S, that is been considered for analysis of our problem is shown in the figure. And, the problem statement is explained in the figure. 
Six states, S1, S2, S3, S4, S5, S6 are taken into consideration and state diagram, state table is made using the following logic explained in the figure. 

### Problem Statement

![Logic_Diagram](https://user-images.githubusercontent.com/83152452/131366783-8c025386-8011-4ef9-a766-d0a07e4244ac.png)

- Green light indicates that there is no traffic and there is easy flow of vehicles in that route/direction. 
- Red light indicates that there is a traffic jam and that route is blocked for the vehicles to move and, 
- Yellow light indicates that the route has medium flow of vehicles. 

Time delays for changing from one state to another is considered as, TMG(from S1 to S2), TY(from S2 to S3), TTG(from S3 to S4), TY(from S4 to S5), TSG(from S5 to S6) 
and TY(from S6 to S1) and the cycle continues.


### State Diagram

![State_Diagram](https://user-images.githubusercontent.com/83152452/131366795-bc45473d-4398-47bb-bad9-a520a779c8bc.png)

In Figure, The time delays are considered as follows :

- TMG = 7 seconds
- TY = 2 seconds
- TTG = 5 seconds
- TSG = 3 seconds

Until TMG seconds, the signal will remain in S1 state, and after TMG seconds, it will move to S2 state. Until TY seconds it will remain in S2 state and after TY seconds, 
it will move to S3 state, and so on. After TY seconds, in state S6, it will go back to S1 state and the cycle continues.


### State Table

![StateTable](https://user-images.githubusercontent.com/83152452/131366804-309b6e9a-4c9c-442b-8753-281a933254f6.png)

In Figure, 
- R = RED, 
- Y = YELLOW and, 
- G = GREEN.

ST = State Transition; A, B and C are considered as the present state.

## design Code

`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 03.03.2025 14:51:23
// Design Name: 
// Module Name: traffic_light_controller
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////

module traffic_light_controller(
    input clk,rst,
    output reg [2:0] light_M1,
    output reg [2:0] light_MT,
    output reg [2:0] light_M2,
    output reg [2:0] light_S,
	 output reg [2:0] count
    );

reg [2:0] p_state;


//local parameters declaration
parameter s1=3'b000;
parameter s2=3'b001;
parameter s3=3'b010;
parameter s4=3'b011;
parameter s5=3'b100;
parameter s6=3'b101;

parameter sec_7=3'b111,sec_5=3'b101,sec_3=3'b011,sec_2=3'b010;

//code block for present state 
always@(posedge clk)
begin
if(rst==1) begin
p_state =s1;
count =3'b000;
end

else begin

case(p_state)
s1: if(count == sec_7)
    begin
	 p_state= s2;
	 count=3'b000;
    end
	 else
	 begin
	 p_state= s1;
	 count= count+3'b001; 
	 end
	 
s2: if(count == sec_2)
	 begin
	 p_state= s3;
	 count=3'b000;
	 end
	 else
    begin
	 p_state= s2;
	 count= count+3'b001;
	 end
	 
s3: if(count == sec_5)
    begin
	 p_state= s4;
	 count=3'b000;
	 end
	 else
	 begin
	 p_state= s3;
	 count= count+3'b001;
	 end
	 
	 
s4: if(count == sec_2)
    begin
	 p_state= s5;
	 count=3'b000;
	 end
	 else
	 begin
	 p_state= s4;
	 count= count+3'b001;
	 end
	 
	 
s5: if(count == sec_3)
    begin
	 p_state= s6;
	 count=3'b000;
	 end
	 else
	 begin
	 p_state= s5;
	 count= count+3'b001;
	 end

s6: if(count == sec_2)
    begin
	 p_state= s1;
	 count=3'b000;
	 end
	 else
	 begin
	 p_state= s6;
	 count= count+3'b001;
	 end
	 

default: begin
         p_state= s1;
         count= 3'b000;
			end
			
endcase
end
end


//code block for output states
always@(p_state)
begin
case(p_state)

s1: begin
    light_M1 <= 3'b001; // Green
	 light_MT <= 3'b100; // Red
	 light_M2 <= 3'b001; // Green
	 light_S  <= 3'b100; // Red
    end
	 
s2: begin
    light_M1 <= 3'b001; //Green
	 light_MT <= 3'b100; //Red
	 light_M2 <= 3'b010; //Yellow
	 light_S  <= 3'b100; // Red
    end
	  
s3: begin
    light_M1 <= 3'b001; // Green
	 light_MT <= 3'b001; //Green
	 light_M2 <= 3'b100; //Red
	 light_S  <= 3'b100; //Red
    end
	 
s4: begin
    light_M1 <= 3'b010; // Yellow
	 light_MT <= 3'b010; // Yellow
	 light_M2 <= 3'b100; //Red
	 light_S  <= 3'b100; // Red
    end
	
s5: begin
    light_M1 <= 3'b100; //Red
	 light_MT <= 3'b100; //Red
	 light_M2 <= 3'b100; //Red
	 light_S  <= 3'b001; // Green
    end
	 
s6: begin
    light_M1 <= 3'b100; //Red
	 light_MT <= 3'b100; // Red
	 light_M2 <= 3'b100; //Red
	 light_S  <= 3'b010; // Yellow 
	 end
	 
default: begin
    light_M1 <= 3'b001;
	 light_MT <= 3'b100;
	 light_M2 <= 3'b001;
	 light_S  <= 3'b100;
    end
	 
endcase
end
endmodule


### RTL Schematic View

![RTL Schematic View](https://user-images.githubusercontent.com/83152452/131369733-eb1964e5-319f-4997-9b3e-6f6ee3e2fb6e.jpeg)


## TESTBENCH

`timescale 1ns / 1ps
//////////////////////////////////////////////////////////////////////////////////
// Company: 
// Engineer: 
// 
// Create Date: 03.03.2025 14:52:16
// Design Name: 
// Module Name: traffic_light_controller_tb
// Project Name: 
// Target Devices: 
// Tool Versions: 
// Description: 
// 
// Dependencies: 
// 
// Revision:
// Revision 0.01 - File Created
// Additional Comments:
// 
//////////////////////////////////////////////////////////////////////////////////
module traffic_light_controller_tb;

	// Inputs
	reg clk;
	reg rst;

	// Outputs
	wire [2:0] light_M1;
	wire [2:0] light_MT;
	wire [2:0] light_M2;
	wire [2:0] light_S;
	wire [2:0] count;
    
	// Instantiate the Unit Under Test (UUT)
	traffic_light_controller uut (
		.clk(clk), 
		.rst(rst), 
		.light_M1(light_M1), 
		.light_MT(light_MT), 
		.light_M2(light_M2), 
		.light_S(light_S), 
		.count(count)
	);

initial
 begin
  clk=1'b1;
  forever #5 clk=~clk;
 end
 
initial
 begin
  rst=1'b1;
  #15;
  rst=1'b0;
  #1000;
  $stop;
 end

initial begin
$monitor("time=%g, rst=%b, count=%b, light_M1=%b, light_MT=%b, light_M2=%b, light_S=%b",$time,rst,count,light_M1,light_MT,light_M2,light_S);
end     
endmodule


## Output Waveforms


![Output_waveform-1](https://github.com/praveen132-cpu/TRAFFIC-LIGHT-CONTROLLER-USING-VERILOG-HDL/blob/main/simulation%20result.jpg?raw=true)


## Result

In this model we have observed various stages which describes about every signals. For example, Consider that at first stage (north-south end) signals gives some indication. 
Then, the signal is red that means signal at east-west side gives a green indication and traffic moves to their respective direction. Then after some delay yellow signal is 
obtain at east-west side and after the red signal arrives at the same time at the north-south end red signal goes off and green signal gets on and traffic moves to their 
particular direction. In this way process continues in the loop every day. 

The modern ways of multi-way traffic management improves the traffic condition up to a large extent. Traffic intensity is sensed and accordingly time is allotted for traffic to
pass. Verilog HDL is used to circuit description, code is generated which is simulated using Xilinx14.5.

This traffic light control system works on the concept of fixed time allocation at each side of the junction which cannot be changed as per varying traffic density. 
Timings allotted at every junction are fixed. Sometimes higher traffic density at one side of the junction demands longer time duration for green signal compared to the 
standard allotted time.

Thus, traffic light control system helps to conduct orderly flow of vehicles. There are lot many issues of obstacles, high level accidents which occurs every day. 
So, traffic signal controller prevents such occurrences. Still many areas or small towns don’t have the traffic light control facilities. And thus, many accident problems occur 
at those areas. Therefore, it is a primary purpose to have such facility in order to control and maintain the area.


## Future Work

This project can be enhanced in such away as to control automatically the signals depending on the traffic density on the roads using sensors like IR detector/receiver module 
extended with automatic turn off when no vehicles are running on any side of the road which helps in power consumption saving. 

A lot of development ideas for work in future can be implemented, such as using solar energy (independent power supply, i.e. saving the power). Using the GPRS map as an 
additional step for development and choosing the best road for the emergency and police vehicles. For national highways we can also design the 8 road traffic light controllers.


## References

1.	Cao, K. (2025). Traffic Light Controller System Analysis based on FPGA. Applied and Computational Engineering, 124(1), 199–211. https://doi.org/10.54254/2755- 2721/2025.20027
2.	Broydé, F., & Clavelier, E. (2025). About the Gains and the Effective Areas of a Multiport Antenna Array. https://doi.org/10.5281/zenodo.14584381
3.	Zhang, L.-K., Yang, Y., Zhou, S.-G., Xu, C., Feng, Y., Yang, Y., & Li, J. (2025).
Wideband High-Gain Dual-Polarized Array Antenna with Bottom-Coupled Feeding Structure. IEEE Transactions on Antennas and Propagation, 1. https://doi.org/10.1109/tap.2024.3523678
4.	Liu, H., Wang, H., Xu, L., Liu, B., Liu, J., Zhang, X., Yuan, X., & Li, B. (2025). A Fast Electromagnetic Radiation Simulation Tool for Finite Periodic Array Antenna and Universal Array Antenna. International Journal of Rf and Microwave Computer-Aided Engineering, 2025(1). https://doi.org/10.1155/mmce/5999155
5.	Xu, H., Zhou, G., Wong, K., New, W. K., Wang, C., Chae, C., Murch, R., Jin, S., & Zhang,
Y. (2024). Channel Estimation for FAS-Assisted Multiuser mmWave Systems. IEEE Communications Letters, 28(3), 632–636. https://doi.org/10.1109/lcomm.2023.3347951
6.	Yuan, S., Wang, J., Xu, H., Wang, T., Li, D., Chen, X., Huang, C., Sun, S., Zheng, S., Zhang, X., Li, E., & Wang, S. (2024). Breaking the Degrees-of-Freedom Limit of Holographic MIMO Communications: A 3-D Antenna Array Topology. IEEE Transactions on Vehicular Technology, 1–13. https://doi.org/10.1109/tvt.2024.3372704
7.	Hu, G., Wu, Q., Ouyang, J., Xu, K., Cai, Y., & Al‐Dhahir, N. (2024). Movable-Antenna- Array-Enabled Communications with CoMP Reception. IEEE Communications Letters,
1. https://doi.org/10.1109/lcomm.2024.3358762

8.	Takizawa, Y., & Fukasawa, A. (2024). Compact-High Performance Microwave Circularly Polarized Antenna Array at Lower Frequency Band. 649–650. https://doi.org/10.1109/ap- s/inc-usnc-ursi52054.2024.10686358
9.	Zhu, Y., Ma, W., Wang, C., & Cao, W. (2024). Low sidelobe planar electrically large sparse array antenna with element number reduction based on genetic algorithm. Iet Microwaves Antennas & Propagation. https://doi.org/10.1049/mia2.12475
10.	Wang, C., & Lee, J.-H. (2024). Generalized Sidelobe Canceller Based Antenna Array
 
Beamformer with Reduced Computational Complexity (pp. 845–856). Springer International Publishing. https://doi.org/10.1007/978-3-031-47724-9_56
11.	Murasing, F. H. (2024). Advancements in antenna array design for 5g communication networks: a comprehensive review. International Journal of Scientific Research in Modern Science and Technology, 3(2), 01–14. https://doi.org/10.59828/ijsrmst.v3i2.181
12.	Wang, P. P., Khormuji, M. N., & Popović, B. M. (2024). Beamforming Performances of Holographic Surfaces. IEEE Transactions on Wireless Communications, 1.
13.	Kheder, R., Ghayoula, R., Smida, A., Gmati, I., Latrach, L., Amara, W., Hammami, A., Fattahi, J., & Waly, M. (2024). Enhancing Beamforming Efficiency Utilizing Taguchi Optimization and Neural Network Acceleration. Telecom, 5(2), 451–475. https://doi.org/10.3390/telecom5020023
14.	Li, W., Yin, H., Qin, Z., & Debbah, M. (2024). Wavefront Transformation-based Near- field Channel Prediction for Extremely Large Antenna Array with Mobility. IEEE Transactions on Wireless Communications, 1. https://doi.org/10.1109/twc.2024.3432333
15.	Cheng, X., & Shao, H. (2024). Fast Simulation of Antenna Array with Parameters Variation. https://doi.org/10.1109/iccem60619.2024.10558966
16.	Kim, Y.-B., & Lee, H. (2024). Compact planar phased array antenna for extended V2X communication coverage. Alexandria Engineering Journal, 94, 226–234. https://doi.org/10.1016/j.aej.2024.03.054
17.	Xiang, L., Pei, F., & Klein, A. (2024). Joint Optimization of Beamforming and 3D Array Steering	for	Multi-Antenna	UAV	Communications. https://doi.org/10.1109/wcnc57260.2024.10570943
18.	Geise, R., Neubauer, B., Weiss, A. K. H., & Akar, A. (2024). On the Imaging of Large Antenna Array Navigation Systems. Transactions of The Japan Society for Aeronautical and Space Sciences, 67(3), 119–126. https://doi.org/10.2322/tjsass.67.119
19.	Ren, A., Song, W., Zhang, Z., Cui, W., Zhang, X., & Yang, L. (2024). A High Isolation Ultra-Wideband Eight-Element MIMO Antenna Pairs Array for 5G Mobile Terminals. 1–
2. https://doi.org/10.1109/aces-china62474.2024.10699639

20.	Cheng, M., Xie, Y., Huang, Z., Zhang, M., & Wu, Y. (2024). Coded Caching Scheme for Partially Connected Linear Networks Via Multi-antenna Placement Delivery Array. IEEE Transactions on Communications, 1. https://doi.org/10.1109/tcomm.2024.3416888
 
21.	Prilutskiy, A. A. (2024). Use of the Sub Array From Microstrip Elements in Dielectric Layers Before the Aperture of Phased Antenna Array for Impedance Matching at Scanning. Антенны. https://doi.org/10.18127/j03209601-202401-05
22.	Mackay, A., & Eleftheriads, G. V. (2024). Mixer-fed Antenna Array with Full Scanning and Sidelobe Control. IEEE Transactions on Antennas and Propagation, 1. https://doi.org/10.1109/tap.2024.3486962
23.	Molchanov, P. (2024). Evolution of Antenna Array Systems (pp. 23–52). River Publishers. https://doi.org/10.1201/9781003476559-2
24.	Pant, M., & Malviya, L. (2024). High Gain and Low ECC MIMO Antenna Array for Millimeter Wave Communication Systems. https://doi.org/10.21203/rs.3.rs-4348523/v1
25.	Zhao, S., Chen, L., Pan, J., & Zhang, T. (2024). A Wide-Angle and PON Fully Polarimetric Retrodirective Array at the X Band. Micromachines, 15(12), 1418. https://doi.org/10.3390/mi15121418
26.	Yu, H., Shang, X., Xue, Q., Ding, H., Wang, J., Lv, W., & Luo, Y. (2024). Twelve- Element MIMO Wideband Antenna Array Operating at 3.3 GHz for 5G Smartphone Applications. Electronics, 13(18), 3585. https://doi.org/10.3390/electronics13183585
27.	Obermaier, M., Lange, J., Deckert, T., Vanden Bossche, M., & Plettemeier, D. (2024). RX Characterization of mmWave Antenna Arrays using an Active Probe Array Structure. 704–707. https://doi.org/10.23919/eumc61614.2024.10732256
28.	m, C., Youn, S., Lim, T. H., & Choo, H. (2024). Design of a Compact Log Periodic Dipole Array Antenna for Broadband and High-Power Beam Synthesis Using Superposition. Journal of Electromagnetic Engineering and Science, 24(3), 234–242. https://doi.org/10.26866/jees.2024.3.r.224
29.	Peshkov, I. W. (2024). Simulation of a Two-Element Low-Complexity Hybrid Antenna Array for DOA-Estimation with Increased Accuracy. 1–5. https://doi.org/10.1109/synchroinfo61835.2024.10617790
30.	Durmuş, A., Yıldırım, Z., Kurban, R., & Karaköse, E. (2024). An optimal concentric circular antenna array design using atomic orbital search for communication systems. Frequenz, 0(0). https://doi.org/10.1515/freq-2023-0432
31.	Monares, K. A., Ebones, I. R., & Arboleda, E. (2024). Literature Review on Milimeter- Wave	Antenna	Array	Designs	for	5G	Communication.
 
https://doi.org/10.20944/preprints202405.0714.v1

32.	Monares, K. A., Ebones, I. R., & Arboleda, E. (2024). Literature Review on Milimeter- Wave Antenna Array Designs for 5G Communication. https://doi.org/10.20944/preprints202405.0714.v1



## Author

- [A Devipriya](https://github.com/Devipriya1921), Bachelor of Engineering in Electronics and Communication Engineering, Cambridge Institute of Technology, Bangalore

    
