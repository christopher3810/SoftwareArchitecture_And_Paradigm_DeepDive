
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

![weather_warning_system](https://github.com/user-attachments/assets/9a965f1e-1e09-4687-8e3c-7b194d6dea66)

### Development Sequence
---

![sequence](https://github.com/user-attachments/assets/6dd01415-19ef-43b3-9830-380b91a8d11d)

Detour for type-checked languages: Provided & Required interfaces

![Provided   Required interfaces](https://github.com/user-attachments/assets/aa7e2d78-d7ca-4b52-906f-dc5d466e0790)
