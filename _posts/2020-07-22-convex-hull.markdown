---
layout: post
title:  "Convex Hull: Algorithm and implementation in C++"
date:   2020-07-22 14:19:41 -0400
categories: Algorithms
---

Convex Hull is among the most widely used algorithms in computational geometry.
The goal of the algorithm is simple:
*Find the minimal set of points to represent a convex polygon that encloses all points in the space.*


![convex hull diagram](https://cdncontribute.geeksforgeeks.org/wp-content/uploads/ConvexHull.png)


In this example, I will implement a convex hull using *Graham Scan*, which is the simplest method to understand.
Here are the main steps necessary for the algorithm:

1. Convert points from rectangular to polar form
2. Sort the points in increasing order by the angle they make with the origin, &theta;
3. Apply Graham Scan to yield the points that form a hull around the shape

{% highlight cpp %}

#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>
#include <stack>
using namespace std;

struct Point {
    double theta, radius;
    double x, y;

    //convert points from rectangular to polar coordinates
    Point(double x, double y) {
        this->x = x;
        this->y = y;
        radius = sqrt( pow(x,2) + pow(y,2) );
        theta = atan(y/x);
    }
};

bool compare_points(Point& p1, Point& p2) {
    if (p1.theta < p2.theta) return true;
    else if (p1.theta == p2.theta && p1.radius < p2.radius) return true;
    return false;
}

int main() {

    //an example set of points in rectangular coordinates
    double rectangular_points[][2] = {
        {1,3},
        {2,5},
        {2,1},
        {1,6},
        {4,6},
        {3,3}
    };

    //construct a set of points in polar form
    vector<Point> points;
    for (int i = 0; i < sizeof(rectangular_points)/sizeof(rectangular_points[0]); i++) {
        points.push_back(Point(rectangular_points[i][0],rectangular_points[i][1]));
    }

    //now sort the points in increasing angle theta
    sort(points.begin(), points.end(), compare_points);

    stack<Point> convex_hull;
    for (vector<Point>::iterator it = points.begin(); it != points.end(); ++it) {
        if (!convex_hull.empty() && (*it).radius > convex_hull.top().radius)
            convex_hull.pop();
        convex_hull.push(*it);
    }

    while (!convex_hull.empty()) {
        Point p = convex_hull.top();
        convex_hull.pop();
        cout << "point " << p.x << " " << p.y << endl;
    }

}

{% endhighlight %}

If you'd like to learn more about Convex Hull and other algorithms, I highly recommend this book:  [The Algorithm Design Manual](http://www.algorist.com/)