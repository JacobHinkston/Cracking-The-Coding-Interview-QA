<h2>16.3 Intersection:</h2>
<p>
    Given two straight line segments (represented as a start point and an end point), compute the point of intersection, if any.
</p>
<h3>Solution:</h3>
<p>We first need to think about what it means for two line segments to intersect.</p>
<p>
    For two infinite lines to intersect, they only have to have different slopes. If they have the same slope, then they must be the exact same line (same y-intercept).That is:
</p>
<code>
    slope 1 != slope 2 <br/>
    OR <br/>
    slope 1 slope 2 AND intersect 1 == intersect 2 <br/>
</code>
<p>
    For two straight lines to intersect, the condition above must be true, plus the point of intersection must be within the ranges of each line segment.
</p>
<code>
    extended infinite segments intersect <br/>
    AND <br/>
    intersection is within line segment 1 (x and y coordinates) <br/>
    AND <br/>
    intersection is within line segment 2 (x and y coordinates)
</code>
<p>
    What if the two segments represent the same infinite line? In this case, we have to ensure that some portion of their segments overlap. If we order the line segments by their x locations (start is before end, point 1 is before point 2), then an intersection occurs only if:
</p>
<code>
    Assume:<br/>
    startl.x < start2.x && startl.x < endl.x && start2.x < end2.x <br/>
    Then intersection occurs if: <br/>
    start2 is between startl and endl <br/>
</code>
<p>
    We can now go ahead and implement this algorithm.
</p>

```java
Point intersection(Point start1, Point end1, Point start2, Point end2) {
    /* 
        Rearranging these so that, in order of x values: start is before end and
        point 1 is before point 2. This will make some of the later logic simpler. 
    */
    if (startl.x > endl.x) swap(startl, endl);
    if (start2.x > end2.x) swap(start2, end2);
    if (startl.x > start2.x) {
      swap(start1, start2);
      swap(end1, end2);
    }

    /* Compute lines (including slope and y intercept) */
    Line line1 = new Line(start1, end1);
    Line line2 = new Line(start2, end2);
    if (line.slope == line2.slope) {
        if (linel.yintercept == line2.yintercept && 
            isBetween(startl, start2, end1)
        ){ 
            return start2;
        }
        return null;
    }

    /*
        If the lines are parallel, they intercept only if they have the same y * intercept and start 2 is on line 1.
        Get intersection coordinate.
    */
    double x = (line2.yintercept - linel .yintercept) / (linel.slope - line2.slope); 
    double y = x * linel.slope + linel.yintercept;
    Point intersection = new Point(x, y);

    /* Check if within line segment range. */
    if (isBetween(startl, intersection, endl) && 
        isBetween(start2, intersection, end2)) {
            return intersection; 
        }
        return null; 
    }

    /* Checks if middle is between start and end. */
    boolean isBetween(double start, double middle, double end) {
        if (start > end) {
            return end <= middle && middle <= start;
        } else {
            return start <= middle && middle <= end;
        }
    }

    /* Checks if middle is between start and end. */
    boolean isBetween(Point start, Point middle, Point end) { 
        return isBetween(start.x, middle.x, end.x) &&
            isBetween(start.y, middle.y, end.y);
    }

    /* Swap coordinates of point one and two. */
    void swap(Point one, Point two) { 
        double x = one.x;
        double y = one.y;
        one.setLocation(two.x, two.y);
        two.setLocation(x, y);
    }

    public class Line { 
        public double slope, yintercept;
        public Line(Point start, Point end) {
            double deltaY = end.y - start.y;
            double deltaX = end.x - start.x;
            slope = deltaY / deltaX; // Will be Infinity (not exception) when deltaX = θ 
            yintercept = end.y - slope * end.x;
        }
    }

    public class Point {
        public double x, y;
        public Point(double x, double y) {
            this.x x;
            this.y = y;
        }
        public void setLocation(double x, double y) {
            this.x x;
            this.y = y;
        }
    }
```
<p>
    For simplicity and compactness (it really makes the code easier to read). we've chosen to make the variables within Point and Line public. You can discuss with your interviewer the advantages and disadvantages of this choice. 
</p>