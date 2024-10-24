
### What is the point? 
---

Create your application to work without either a UI or a database so you can run automated regression-tests against it, work when the database becomes unavailable, upgrade to new technology, and link applications together.

### Benefits
---

1. You get to decide the app’s driven actors at initialization, over a period of years as technologies shift, or in real time. 
2. You get to replace production connections with test harnesses, and back again, without changing the source code. 
3. You get to avoid having to change the source code and then rebuild the system every time you make these shifts. 
4. You can prevent leaks of business logic into the UI or data services, and vice versa, prevent leaks of UI or data service logic into the business logic.

### Costs 
---

1. You must add an instance var to hold each driven actor, or get it every time. 
2. You must add a constructor parameter or a setter function for each driven actor, or a call to the configurator to get it. 
3. You must design and add a configurator. 
4. (Type-checked languages) You must declare the “required” interfaces. 
5. (Type-checked languages) You must add folder structure for the port declarations.


### Why? When would it have been worth it?
---

![why_when_workth](https://github.com/user-attachments/assets/89602934-b67f-48f5-9d47-cc55162d7ad4)

The app is a “component”

![app_with_port](https://github.com/user-attachments/assets/159079f2-c12d-4195-bfd9-d454576ee575)

The app is a component, with ports

Hooking up the component for testing

Hooking up the component for production

![weader_system_example](https://github.com/user-attachments/assets/29526f7f-de61-4964-bf80-d1269b55a0fc)

### Development Sequence
---

![sequence](https://github.com/user-attachments/assets/6dd01415-19ef-43b3-9830-380b91a8d11d)

Detour for type-checked languages: Provided & Required interfaces

![Provided   Required interfaces](https://github.com/user-attachments/assets/aa7e2d78-d7ca-4b52-906f-dc5d466e0790)

Java example

![java_example](https://github.com/user-attachments/assets/5d529f5f-a3c7-4195-be01-007d8d320a3c)

![java_example_2](https://github.com/user-attachments/assets/63747bad-a23c-47f5-8767-2fb0180b95f8)

### more complexity example
---
![blue_zone_example_1](https://github.com/user-attachments/assets/57cc243b-ff2a-4eab-95a7-48f891d3208a)

![blue_zone_example_2](https://github.com/user-attachments/assets/322aa764-aa82-4cdc-863d-0c20c233d831)

![blue_zone_example_3](https://github.com/user-attachments/assets/75f80808-5682-42cd-9dd9-95c6ca968506)

![blue_zone_example_4](https://github.com/user-attachments/assets/cb11cf17-b56b-46c5-8f55-8bc537fc6434)

![blue_zone_example_5](https://github.com/user-attachments/assets/b177db73-bc2e-4375-8402-505c34d5acea)

### How do we design the configurator?
---

![design_Setter](https://github.com/user-attachments/assets/82dc08e9-6d6a-40db-a858-3fb2da976b0f)

![design_repository_broker](https://github.com/user-attachments/assets/bb05aeb7-6b80-4f1a-807c-15f91229c549)

### Benefits 
---

1. You get to set the app’s driven actors during execution -- at initialization, over a period of years as technologies shift, or in real time. 
2. You get to replace production connections with test harnesses, and back again, without changing the source code. 
3. You get to avoid having to change the source code and then rebuild the system every time you make these shifts. 
4. You can prevent leaks of business logic into the UI or data services, and vice versa, prevent leaks of UI or data service logic into the business logic.

### Costs
---

1. You must add an instance var to hold each driven actor, or get it every time. 
2. You must add a constructor parameter or a setter function for each driven actor, or a call to the configurator to get it. 
3. You must design and add a configurator. 
4. (Type-checked languages) You must declare the “required” interfaces. 
5. (Type-checked languages) You must add folder structure for the port declarations.