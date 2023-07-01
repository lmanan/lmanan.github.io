(From [Chui and Rangarajan, 2003](https://www.sciencedirect.com/science/article/pii/S1077314203000092))
> "There are two unknown variables in the point matching problem - the correspondence and the transformation. While solving for either variable without information regarding the other is quite difficult, an interesting fact is that solving for one variable once the other is known is much simpler than solving the original, coupled problem."


While surveying general methods to perform coarse registration for a pair of images, I came across the classical [Method of Moments](https://www.sciencedirect.com/science/article/pii/016502708890129X) - which tries to solve for the transformation without ever introducing the notion of correspondence. In its traditional form, it computes the second-order moments of the pixel intensity of a two-dimensional image which can be collectively represented as a moment of inertia matrix. The eigen vectors of such a matrix can indicate the principal axes of the image - information, which may come in handy to align the image later. 

$$ 
I= \left( \begin{array}{cc} I_{xx} & I_{xy} \\ I_{yx} & I_{yy} \end{array} \right) 
$$
