# multimodal-transportation-optimization
A project on using mathematical programming to solve multi-modal transportation cost minimization in goods delivery and supply chain management.
## Project Overview
In delivery services, many different transportation tools such as trucks, airplanes and ships are available. Different choices of routes and transporation tools will lead to different costs. To minimize cost, we should consider goods consolidation (Occassions when different goods share a journey together.), different transportation costs and delivery time constraints etc. This project uses mathematical programming to model such situation and solves for cost minimization solution. The model is implemented with **IBM Cplex API** and **numpy** matrixing in Python.

<p align="center"><img src="https://user-images.githubusercontent.com/30411828/45585955-c6311e80-b920-11e8-95c9-bc90089446b4.jpg"></p>

## Assumptions
Before model building, some assumptions should be made to simplify the case because real-world delivery problems consist of too many unmeasurable factors that can affect the delivery process and final outcomes. Here are the main assumptions:<br>
1. The delivery process is **deterministic**, no random effect will appear on delivery time and cost etc. 
2. Goods can be transported in **normal container**, no special containers (refrigerated, thermostatic etc.) will be needed.
3. Container only constraints on the good's **volume**, and all goods are **divisible in terms of volume**. (No bin packing problem needed to be considered.) 
4. The model only evaluates the **major carriage routes**. The first and last mile between end user and origin/destination shipping point are not considered. (**From warehouse to warehouse**.)
5. There is **only one transportation tool available between each two ports**. For instance, we can only directly go from one airport to the other airport in different cities by flight, while direct journey by ship or railway or truck is infeasible.
6. Overall cost is restricted to the most important 3 parts, **transportation cost**, **warehouse cost** and **goods tariff**.

## Decision Variables
In order to fit such problem into the framework of mathematical programming and simplify the later model building part, we need to use the concept of **variable matrix**, a list of variables deployed in the form of a matrix or multi-dimensional array. In our modelling, 3 variable matrices will be introduced.<br>
1. **Decision Variable Matrix: X<sub>i,j,t,k</sub>**<br>
The most important variable matrix in the model. It's a 4 dimensional matrix, each dimension representing start port, end port, time and goods respectively. Each element in the matrix is a binary variable, representing whether a route is taken by a specific goods. For example, element **X[i,j,t,k]** represents whether **goods k** travels from **port i** to **port j** at **time t**.
```python
  self.var = self.binary_var_list(self.portSpace*self.portSpace*self.dateSpace*self.goods,name='x')
  self.x = np.array(self.var).reshape(self.portSpace,self.portSpace,self.dateSpace,self.goods)
```

2. **Container Number Matrix: Y<sub>i,j,t</sub>**<br>
A variable matrix used to support the decision variable matrix. It's a 3 dimensional matrix, with each dimension representing start port, end port and time respectively. Each element in the matrix is an integer variable, representing the number of containers needed in a specific route. For example, **Y[i,j,t]** represents the number of containers needed to load all the goods travelling simultaneously from **port i** to **port j** at **time t**. Such matrix is introduced to make up for the limitation of "linear operator only" in mathematical programming, when we need a **roundup()** method in direct calculation of the container number.
```python
  self.var_2 = self.integer_var_list(self.portSpace*self.portSpace*self.dateSpace,name='y')
  self.y = np.array(self.var_2).reshape(self.portSpace,self.portSpace,self.dateSpace)
```

3. **Route Usage Matrix: Z<sub>i,j,t</sub>**<br>
A variable matrix used to support the decision variable matrix. It's a 3 dimensional matrix, with each dimension representing start port, end port and time respectively. Each element in the matrix is a binary variable, representing whether a route is used or not. For instance, **Z[i,j,t]** represents whether the route from **port i** to **port j** at **time t** is used or not (no matter which goods). It's introduced with similar purpose to **Y<sub>i,j,t</sub>**.
```python
  self.var_3 = self.binary_var_list(self.portSpace*self.portSpace*self.dateSpace,name='z')
  self.z = np.array(self.var_3).reshape(self.portSpace,self.portSpace,self.dateSpace)        
```

## Parameters
Similar to the decision variables mentioned above, the following parameters or parameter matrices are introduced for the sake of later model building:<br>
1. **Transportation Cost: C<sub>i,j,t</sub>**

## Mathematical Modelling

## Optimization Result & Solution

## Model Use & Extension Guide
