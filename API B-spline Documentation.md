# API B-Spline Documentation

## Getting Started

    1. Install Octave.:  https://www.gnu.org/software/octave/ 
    2. Download the nurbs Pacakge from:  http://octave.sourceforge.net/nurbs/index.html
    3. At Octave Command Prompt type   ( Same directory as the downloaded file)
        ..1. pkg install nurbs-<version>.tar.gz
        ..2. pkg load nurbs


## B-Spine

    Given n+1 control points P0,P1,....Pn and a knot vector U = {u0,u1,....um},
    The B-spline curve of a degree is defined by

![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "B-Spline Definition")
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "B-Spline")

    (The black dots are the control points.)

    Given control points and knot vector the given definition will generate a smooth curve.
    That curve would be used to optimize the route of the hyperloop.
    Knot vector are used to control the amount of points affected near a particular control point due to a change in it. Uniform knot vector has same number of affected control points for change in any control point.  

![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Uniform Knot Vector")
 
    Non-Uniform control points result in variation of that number.

![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Non Uniform Knot Vector")

## Functions

    1. Use bspeval to compute the curve.
    2. Use bspderiv to compute the control points and the knot vector of the derivative.
    3. Use kntuniform to compute a uniform knot vector.


###  p = bspeval(d,c,k,u);

    
    Functioning 
    
    * First computes the equation of the B-Spline with the given parameters
    * Then computes the coordinates at the given u values.

    Inputs :

       d - Degree of the B-Spline.
       c - Control Points, matrix of size (dimension ,number of control points).
       k - Knot sequence, row vector of size nk.
       u - Parametric evaluation points, row vector of size nu.

    Output:

       p - Evaluated points

    Important points 

        * The value of d usually lies between 3 & 5 (the k value). 
        * U =   0:0.0001:1  (Add more decimals to get a finer curve).
        * Number of control points (n) + degree of spline = size of knot vector - 1

    Example Code:

    %Example Inputs 
    d = 3;
    c = [0, 1 , 2 , 3 , 4 ,5 ;   0, 1, -1, 0, -2, 1];
    k =[ 0,0,0,1/5,2/5,2/5,4/5,1,1,1];
    u = 0:0.001:1;
 
    %Executing the function
    p = bspeval(d,c,k,u);
    %Plotting the curve
    figure(1);
    %Plotting the control points 
    plot(c(1,:) ,c(2,:),'rx',"marker","*","color","red");
    hold;
    %Plotting the B-Spline curve
    plot(p(1,:) ,p(2,:));

![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "bspeval graph")



### [dc,dk] = bspderiv(d,c,k)

    Functioning 
    * First computes the derivative of the B-Spline with the given parameters
    * Then computes the control points and the knot vector of the derived curve.
    
    Inputs :

       d - Degree of the B-Spline.
       c - Control Points, matrix of size (dimension ,number of control points).
       k - Knot sequence, row vector of size nk.

    Output:

      dc - control points of the derivative         double  matrix(mc,nc)
      dk - knot sequence of the derivative       double  vector(nk)


     Important points 

    * The value of d usually lies between 3 & 5 (the k value).
    * Number of control points (n) + degree of spline = size of knot vector - 1
    * Degree decreases every time you take a derivative. 


    Example Code:

    %Example Inputs 
    d = 4;
    c = [0, 0, 0, 0, 1, 0, 0, 0, 0];
    k =[ 0, 0, 0, 0, 0, 1/5, 2/5, 3/5, 4/5, 1,1,1,1,1];
    u = 0:0.001:1;
 
    %Executing the function
    p = bspeval(d,c,k,u);
 
    %Calculating the First derivative 
 
    [fdc,fdk] = bspderiv(d,c,k);
    fdc = fdc/max(fdc);
    fp = bspeval(d-1,fdc,fdk,u);
  
    %Calculating the Second derivative 
 
    [sdc,sdk] = bspderiv(d-1,fdc,fdk);
    sdc = sdc/max(sdc);
    sp = bspeval(d-2,sdc,sdk,u);

    %Plotting the curve
 
    figure(1);
    %Plotting the B-Spline curve
    plot(u,p,';Spline;');
    hold;
    %Plotting the first Derivative curve
    plot(u,fp,'color','green',';1st derivative;');
    %Plotting the second Derivative curve
    plot(u,sp,'color','red',';2nd derivative;');


![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "bspderiv graph")



### [csi, zeta] = kntuniform (num, degree, regularity)


    Functioning 

    * Computes the uniform knot vector in the given reference domain.


    Inputs:

     num:            number of breaks (in each direction)
     degree:        polynomial degree (in each direction)
     regularity:     global regularity (in each direction)
	
    Output:

     csi:  knots
     zeta: breaks = knots without repetitions


    Important points 

    * Regularity  should be > degree 
    * Breaks - number of partition between 0 & 1.
    * Regularity controls the number of repetition of a values in the  knot vector.
    * Input can be given for a single case or multiple cases at the same time.



    Example Code:

    1.  num = 3;
    degree = 3;
    regularity =  1;
    [csi, zeta] = kntuniform (num, degree, regularity)

    csi =   0.00000   0.00000   0.00000   0.00000   0.50000   0.50000   1.00000   1.00000   1.00000   1.00000
    zeta =   0.00000   0.50000   1.00000

    num = 4;
    degree = 4;
    regularity =  2;
    [csi, zeta] = kntuniform (num, degree, regularity)
    
    csi =   0.00000   0.00000   0.00000   0.00000   0.00000   0.33333   0.33333   0.66667   0.66667   1.00000   1.00000   1.00000  1.00000   1.00000
    zeta = 0.00000   0.33333   0.66667   1.00000

    For further details:

    zeta{idim} = linspace (0, 1, num(idim));
    rep  = degree(idim) - regularity(idim);
    if (rep > 0)
        csi{idim}  = [zeros(1, degree(idim)+1-rep)...
          reshape(repmat(zeta{idim}, rep, 1), 1, []) ones(1, degree(idim)+1-rep)];
    else
    error ('kntuniform: regularity requested is too high')
    end



### For Plotting the data

    * Use plot(x,y) to plot in 2D.
    * Use plot3(x,y,z) to plot in 3D.
    * Use ‘hold’ to draw without erasing.













