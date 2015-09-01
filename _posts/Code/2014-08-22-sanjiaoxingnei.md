---
layout: post
title: 判断点是否在三角形内
categories: code
---

[资料1](http://www.cnblogs.com/graphics/archive/2010/08/05/1793393.html)
[资料2](http://www.cnblogs.com/neoragex2002/archive/2007/09/09/887556.html)

<!--more-->
- 同向法

      c#
  // 3D vector
  class Vector3
  {
  public:
      Vector3(float fx, float fy, float fz)
          :x(fx), y(fy), z(fz)
      {
      }
  
      // Subtract
      Vector3 operator - (const Vector3& v) const
      {
          return Vector3(x - v.x, y - v.y, z - v.z) ;
      }
  
      // Dot product
      float Dot(const Vector3& v) const
      {
          return x * v.x + y * v.y + z * v.z ;
      }
  
      // Cross product
      Vector3 Cross(const Vector3& v) const
      {
          return Vector3(
              y * v.z - z * v.y,
              z * v.x - x * v.z,
              x * v.y - y * v.x ) ;
      }
  
    public:
        float x, y, z ;
    };
    
    // Determine whether two vectors v1 and v2 point to the same direction
    // v1 = Cross(AB, AC)
    // v2 = Cross(AB, AP)
    bool SameSide(Vector3 A, Vector3 B, Vector3 C, Vector3 P)
    {
        Vector3 AB = B - A ;
        Vector3 AC = C - A ;
        Vector3 AP = P - A ;
    
        Vector3 v1 = AB.Cross(AC) ;
        Vector3 v2 = AB.Cross(AP) ;
    
        // v1 and v2 should point to the same direction
        return v1.Dot(v2) >= 0 ;
    }
    
    // Same side method
    // Determine whether point P in triangle ABC
    bool PointinTriangle1(Vector3 A, Vector3 B, Vector3 C, Vector3 P)
    {
        return SameSide(A, B, C, P) &&
            SameSide(B, C, A, P) &&
            SameSide(C, A, B, P) ;
    }
    

- 重心法

  ```c#
     // Determine whether point P in triangle ABC
    bool PointinTriangle(Vector3 A, Vector3 B, Vector3 C, Vector3 P)
    {
        Vector3 v0 = C - A ;
        Vector3 v1 = B - A ;
        Vector3 v2 = P - A ;
    
        float dot00 = v0.Dot(v0) ;
        float dot01 = v0.Dot(v1) ;
        float dot02 = v0.Dot(v2) ;
        float dot11 = v1.Dot(v1) ;
        float dot12 = v1.Dot(v2) ;
    
        float inverDeno = 1 / (dot00 * dot11 - dot01 * dot01) ;
    
        float u = (dot11 * dot02 - dot01 * dot12) * inverDeno ;
        if (u < 0 || u > 1) // if u out of range, return directly
        {
            return false ;
        }
    
        float v = (dot00 * dot12 - dot01 * dot02) * inverDeno ;
        if (v < 0 || v > 1) // if v out of range, return directly
        {
            return false ;
        }
    
        return u + v <= 1 ;
    }
    ```
