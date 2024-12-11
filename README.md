java c
ELEC0021: MATLAB AND SIMULINK BASICS FOR CONTROL SYSTEMS DESIGN
1. INTRODUCTION
A control systems CAD package aids control systems design by automatically calculating the graphical displays that designers use. It does all the numerical work leaving you free to concentrate on the important design decisions. MATLABreg;   and its Control Systems toolbox provides an ideal environment.
This note gives you an introduction to the basic features of the Control Systems toolbox. You are urged to become familiar with the material outlined in this note because it will be of help in the laboratory exercise as well as in the study of the material in the problem sheets.
2. GETTING STARTED
Launching MATLAB will open the Matlab terminal window, as well as several other support windows. The MATLAB prompt symbol is ». For help on any command simply type
» help    
on the terminal. For instance, for information about polynomial roots type
» help roots
MATLAB distinguishes between lower and upper case, and they are not interchangeable. For example, to leave MATLAB type (in lower case):
» quit
3. CONTROL SYSTEMS IN MATLAB
Vectors and arrays:    The basic data structures in MATLAB are vectors and matrices. Square brackets [ ] are used to define vectors. Round brackets ( ), by contrast, are used to indicate the arguments of a function. For example in the statement
» [re, im] = nyquist(num,den,w)
the function nyquist   uses vectors num, den   and w   as data (the arguments) from which to calculate or define vectors re   and im.
Vectors can also be defined recursively, for example the statement below sets up a vector
t=[0    0.1    0.2    0.3    .....    9.8    9.9    10.0]
» t = 0:0.1:10
MATLAB usually echoes the result of a calculation to the screen. If you want to suppress the echo the statement can be ended with a semi-colon, as shown below
» t = 0:0.1:10;
Example - closed loop poles:    A control systems example presented in the lectures is a d.c. motor represented by the following open loop transfer function:
Go(s) = s(s + 1)(s + 10)/50
The numerator polynomial has just one element. The numerator can be given any name you wish; in the example below it is called num:
» num = 50;
The denominator polynomial can be specified either as a vector of roots or else as a vector of polynomial coefficients. MATLAB uses the convention in its Control Systems toolbox that vectors of roots are column vectors and vectors of polynomial coefficients are row vectors. Now, s(s + 1)(s + 10)   is equal to s3 + 11s2 + 10s. The MATLAB routines need the denominator in polynomial form. so we could write:
» den = [1    11 10    0];
If the transfer function is in factored form, however, you can do:
» den = [0
-1
-10];
» den = poly(den);
Try this to establish that the polynomial coefficients are calculated correctly. The inverse of the poly   function is roots   which calculates the roots of polynomials. It is possible to determine the closed loop poles of the d.c. motor using the roots   function. The characteristic equation is 1 + KGo(s), and the closed loop poles are those values of s that satisfy the equation 1 + KGo(s) = 0. That is to say, we need to determine the roots of 1 + KGo(s).
When K   = 1 the polynomial coefficients of 1 + KGo(s)   are [1    11    10    50] and the closed loop poles are therefore determined as:
» dencl = [1    11    10    50];
» clpoles = roots(dencl)
The vector clpoles   contains the roots of the characteristic equation, which are the values of s where the closed loop poles are located on the s-plane. The results are approximately -10.5, and . Make sure you understand the time-domain signals that correspond to these closed loop poles.
It is possible to create a plot on the s-plane showing how the closed loop pole positions change as the gain, K, varies. It is called a root-locus plot and uses the open loop   transfer function (num   and den) to determine the closed loop   poles:
» k = 0:0.5:20;
» rk=rlocus(num,den,k);
» plot(rk,'+'), xlabel('Re s'), ylabel('Im s'), grid
Make sure that you know which of the '+' symbols corresponds to which value of the gain k. The gain varies in steps of 0.5 from 0 to 20. The root locus代 写ELEC0021: MATLAB AND SIMULINK BASICS FOR CONTROL SYSTEMS DESIGNMatlab
代做程序编程语言 starts when k = 0 at the open loop pole positions.
Use of the tf   command: The commands the Control Systems toolbox can use a transfer function description of the system. To create a transfer function called sysol from num   and den   type:
» sysol = tf(num,den)      % omit the semicolon so that you can see the transfer function.
Once the system has been defined using the tf   command the root locus plot can be called like this. Note that when called with no output argument on the left hand side MATLAB automatically plots the results:
» rlocus(sysol,k)
Nyquist, Bode and Nichols plots:    Nyquist and Bode plots are parameterised by angular frequency () so before the functions are called it is necessary to generate the vector of angular frequencies. The logspace   function is suitable. Type help logspace   to determine its operation.
The plots generated in this section are of the open loop   system defined in the transfer function sysol.
» w = logspace(0,2);
» [re, im] = nyquist(num,den,w); 
» re = re(:);    im = im(:);         % See footnote, next page
» plot(re,im,'+'), grid
An alternative way to call the Nyquist plot is:
» nyquist(sysol,w)
MATLAB automatically plots a graph when this form. is in use and it also labels the low frequency end of the plot.   
To generate a Bode plot type:
» w = logspace(-1,2,100);
» [mod, arg] = bode(sysol,w); mod = mod(:); arg = arg(:);
» subplot(211), loglog(w,mod), title('Magnitude plot'), xlabel('w')
» subplot(212), semilogx(w,arg), title('Phase plot'), xlabel('w'), ylabel('deg')
The Control Systems toolbox also calculates gain margin (gm), the phase margin (pm), the angular frequency where the open loop gain is 1 (wcp) and the frequency where the open loop phase is    (wcg):
»   [gm, pm, wcg, wcp] = margin(sysol)
The gain margin is the factor by which the gain must be boosted to give unity gain at the angular frequency wcg. If the gain margin of the open loop   is less than unity then the closed loop   system is unstable.
To create a Nichols plot type
»   nichols(sysol,w);
» ngrid    % ngrid superimposes the M-contours onto the Nichols plot
4. CONTROL SYSTEMS IN SIMULINK 
Simulink is a graphical tool that allows us to simulate feedback control systems.
Open Simulink by simply typing simulink   at the MATLAB prompt. Once Simulink has loaded, create a new model by pressing CTRL+N (or click the “New Model” button). You can begin placing components from the component library by dragging and dropping them in the model space. You can make connections by dragging the connection wires to the input junctions. You can zoom in/out by pressing CTRL+“+” / CTRL+ “–”. You can change the simulation time by setting the value in the related box, and then run your model by pressing the green “run” button.
Components: 
You will only need to be concerned with a small fraction of Simulink's component library. In particular, you should be familiar with the following:
·   “Continuous” library: Integrator, Transfer Fcn
·   “Math Operations” library: Gain, Sum, Trigonometric Function
·      “Signal Routing” library: Mux, used to multiplex signals together in order to plot several on one graph
·   “Sinks” library: Scope (used for viewing the system outout), To Workspace (used to transfer a signal to Matlab) or use the Simulation Data Inspector
·   “Sources” library: Ramp, Sine Wave, Step (which generate the corresponding signals)
Modifications of Components:
You can modify component properties by clicking on them and changing the default settings given by Simulink. For example:
·   double-clicking on the “Gain” component, you can alter the gain parameter (you can also use expressions, such as 10.3/2.5)
·   double-clicking on the “Sum” component you can change the signs to |+ –
·   double-clicking the “Scope” to open the scope output; right-clicking on the scope output and selecting “Autoscale” will automatically scale the output range of the scope
·   double-clicking on the “To Workspace” component and changing the “Save format” to “Array”; after you run the simulation, you will then be getting the outputs in the workspace variable “simout”   
   



         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
