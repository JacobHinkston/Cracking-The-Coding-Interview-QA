<h2>16.13 Bisect Squares</h2>

<p>Given two squares on a two-dimensional plane, find a line that would cut these two squares in half. Assume that the top and the bottom sides of the square run parallel to the x-axis. </p>

<h3>Solution</h3>

<p>Before we start, we should think about what exactly this problem means by a "line:' Is a line defined by a slope and a y-intercept? Or by any two points on the line? Or, should the line be really a line segment, which starts and ends at the edges of the squares? </p>
<p>
    We will assume, since it makes the problem a bit more interesting, that we mean the third option: that the line should end at the edges of the squares. In an interview situation, you should discuss this with your interviewer. 
</p>

<p>
        This line that cuts two squares in half must connect the two middles. We can easily calculate the slope, knowing that slope = (y_2-y_1)/(x_2-x_1) . Once we calculate the slope using the two middles, we can use the same equation to calculate the start and end points of the line segment. 
</p>

<p>In the below code, we will assume the origin (e, e) is in the upper left-hand corner. </p>

```java

public class Square {
    
    public Point middle() { 
        return new Point((this.left + this.right) / 2.0, (this.top + this.bottom) / 2.0); 
    } 
    
    /* Return the point where the line segment connecting mid1 and mid2 intercepts 9 * the edge of square 1. That is, draw a line from mid2 to mid1, and continue it 10 * out until the edge of the square. */
    public Point extend(Point mid1, Point mid2, double size) { 
    /* Find what direction the line mid2 -> midi goes. */ 
    double xdir midl.x < mid2.x ? -1 : 1; 
    double ydir = mid1.y < mid2.y ? -1 : 1;
    /* If mid1 and mid2 have the same x value, then the slope calculation will 17 * throw a divide by 0 exception. So, we compute this specially. */ 
    if (mid1.x == mid2.x) { 
        return new Point(midl.x, mid1.y + ydir * size / 2.0); 
    }
    double slope = (mid1.y - mid2.y) / (mid1.x - mid2.x);
    double xl = 0; 
    double y1 = 0; 

    /* 
        Calculate slope using the equation (y1 - y2) I (xl - x2). 27 * Note : if the slope is "steep" (>1) then the end of the line segment will 28 * hit size I 2 units away from the middle on the y axis. If the slope is 29 * "shallow" ((1) the end of the line segment will hit size I 2 units away 36 * from the middle on the x axis. 
    */
    if (Math.abs(slope) == 1) { 
        xl = mid1.x + xdir * size / 2.0; 
        y1 = mid1.y + ydir * size / 2.0; 
    } else if (Math .abs(slope) < 1) { //shallow slope 
        xl = mid1.x + xdir * size / 2.0; 
        y1 = slope * (xl - mid1.x) + mid1.y; 
    } else { //steep slope 
        y1 = mid1.y + ydir * size / 2.0; 
        xl = (y1 - mid1.y) / slope + mid1.x; 
    } 
    return new Point(x1, y1); 
} 

public Line cut (Square other) {
    /* Calculate where a line between each middle would collide with the edges of 46 * the squares */ 
    Point p1 = extend(this.middle(), other.middle(), this.size); 
    Point p2 = extend(this.middle(), other.middle(), -1 * this.size); 
    Point p3 = extend(other.middle(), this.middle(), other.size); 
    Point p4 = extend(other.middle(), this.middle(), -1 * other.size); 
    
    /* 
        Of above points, find start and end of lines. Start is farthest left (with 53 * top most as a tie breaker) and end is farthest right (with bottom most as 54 * a tie breaker. 
    */ 
    Point start = p1; 
    Point end = p1; 
    Point[] points = {p2, p3, p4}; 
    for (int i = 6; i < points. length; i++) { 
        if (points[i].x < start.x || (
                points[i].x == start.x && pOints[i].y < start.y
            )
        ){ 
            start = points[i]; 
        } else if (points[i].x > end.x || (
                points[i].x == end.x && points[i].y > end.y
            )
        ){ 
            end = points[i]; 
        } 
    }
    return new Line(start, end);
}
```

<p>
    The main goal of this problem is to see how careful you are about coding. It's easy to glance over the special cases (e.g., the two squares having the same middle). You should make a list of these special cases before you start the problem and make sure to handle them appropriately. This is a question that requires careful and thorough testing. 
</p>