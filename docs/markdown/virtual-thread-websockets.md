## Virtual thread support for websockets

---

### Before 3.3

Websocket support in Spring Boot would not automatically take advantage of Virtual Threads

---

### But / Because

- Virtual Threads require less footprint than typically OS threads. 
- Websockets typically are long lived, sometimes existing for hours for one client. 

---

### Therefore

- If a AsyncTaskExecutor is available in the context, it is now registered on the websocket `ChannelRegistration`. 
- If using virtual threads, this will usually be a virtual threads enabled `SimpleAsyncTaskExecutor`, making Websockets virtual thread capable.

---

### Result

Websocket enabled application will use less memory and possibly be able to service more concurrent persistent clients than before.