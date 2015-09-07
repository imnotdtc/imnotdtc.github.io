---
layout: post
title: 判断点是否在三角形内
categories: 算法
---

<!--more-->
- 同向法
  
  假设点P位于三角形内，会有这样一个规律，当我们沿着ABCA的方向在三条边上行走时，你会发现点P始终位于边AB，BC和CA的右侧。我们就利用这一点，但是如何判断一个点在线段的左侧还是右侧呢？我们可以从另一个角度来思考，当选定线段AB时，点C位于AB的右侧，同理选定BC时，点A位于BC的右侧，最后选定CA时，点B位于CA的右侧，所以当选择某一条边时，我们只需验证点P与该边所对的点在同一侧即可。问题又来了，如何判断两个点在某条线段的同一侧呢？可以通过叉积来实现，连接PA，将PA和AB做叉积，再将CA和AB做叉积，如果两个叉积的结果方向一致，那么两个点在同一测。判断两个向量的是否同向可以用点积实现，如果点积大于0，则两向量夹角是锐角，否则是钝角。

        class Vector3{
          public Vector3(float fx, float fy, float fz)
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

