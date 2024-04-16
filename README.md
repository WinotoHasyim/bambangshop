# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1

1. **In the Observer pattern diagram explained by the Head First Design Pattern book, Subscriber is defined as an interface. Explain based on your understanding of Observer design patterns, do we still need an interface (or trait in Rust) in this BambangShop case, or a single Model struct is enough?**<br>
Pada Observer Design Pattern, menggunakan interface untuk suatu observer (subsrcriber) dapat membuat kode lebih fleksibel dan mengikuti prinsip open closed principle. Dengan interface, kita bisa membuat berbagai macam observer yang masing-masing mengimplementasikan interface dengan cara yang berbeda-beda. Jika kita ingin mendefinisikan tipe opserver yang baru, kita tinggal membuatnya tanpa memodifikasi kode yang sudah ada, sehingga sejalan dengan prinsip open closed principle. Penggunaan interface untuk subscriber dalam konteks BambangShop ini tergantung pada requirement aplikasi nantinya. Jika semua subscriber memiliki model yang sama, maka satu struct saja sudah cukup. Tetapi, jika aplikasi akan mempunyai tipe subscriber yang berbeda-beda, maka menggunakan interface akan sangat membantu dalam pengembangan kode. 
<br>

2. **id in Program and url in Subscriber is intended to be unique. Explain based on your understanding, is using Vec (list) sufficient or using DashMap (map/dictionary) like we currently use is necessary for this case?**<br>
Menggunakan Vec sebenarnya sudah cukup, tetapi enggunakan DashMap bisa lebih efisien daripada menggunakan Vec. Dalam Vec, proses pencarian id atau url tertentu membutuhkan waktu linier (O(n)). Dalam DashMap, proses pencariannya dapat dilakukan dalam waktu konstan (O(1)), yang jauh lebih cepat untuk jumlah data yang besar. DashMap juga memungkinkan beberapa threads untuk mengakses data secara concurrent
<br>

3. **When programming using Rust, we are enforced by rigorous compiler constraints to make a thread-safe program. In the case of the List of Subscribers (SUBSCRIBERS) static variable, we used the DashMap external library for thread safe HashMap. Explain based on your understanding of design patterns, do we still need DashMap or we can implement Singleton pattern instead?**<br>
Karena aplikasi BambangShop ini bersifat multi-threaded, maka pemakaian DashMap untuk singleton instance yaitu SUBSCRIBERS lebih cocok. Singleton pattern pada aslinya tidak thread-safe. Jika ada beberapa threads yang ingin mengakses suatu singleton instance secara concurrent, maka akan menyebabkan data race dan outcome yang unpredictable. Dengan menggunakan DashMap, maka pengaksesan data dapat dilakukan oleh beberapa thread secara concurrent tanpa menyebabkan data races
<br>

#### Reflection Publisher-2

1. **In the Model-View Controller (MVC) compound pattern, there is no “Service” and “Repository”. Model in MVC covers both data storage and business logic. Explain based on your understanding of design principles, why we need to separate “Service” and “Repository” from a Model?**<br>
Service dan repository sebaiknya dipisahkan dari model agar sejalan dengan Single Responsibility Principle, yaitu setiap class memiliki satu responsibility yang berbeda dengan lainnya. Hal ini membuat kode kita lebih maintainable, modular, dan reusable.
<br>

2. **What happens if we only use the Model? Explain your imagination on how the interactions between each model (Program, Subscriber, Notification) affect the code complexity for each model?**<br>
Jika kita hanya menggunakan Model yang menghandle data storage dan business logic sekaligus, maka kode kita akan menjadi lebih kompleks karena kedua fungsi tersebut akan digabung menjadi satu kesatuan yang akan menyebabkan tight coupling. Akibatnya, jika ada perubahan dalam salah satu dari kedua fungsi tersebut, akan susah untuk memodifikasinya karena business logic dan data storage nya digabung. Kode kita akan susah untuk di maintain dan juga menjadi kurang fleksibel. Selain itu, tight coupling juga akan menyebabkan proses testing menjadi lebih susah
<br>

3. **Have you explored more about Postman? Tell us how this tool helps you to test your current work. You might want to also list which features in Postman you are interested in or feel like it is helpful to help your Group Project or any of your future software engineering projects.**<br>
Sejauh ini, saya belum terlalu meng-eksplor lebih jauh terkait Postman. Tetapi menurut saya, Postman cukup membantu saya dalam testing API dari berbagai tipe HTTP request seperti GET dan PUT. Ini akan sangat berguna ketika group project nanti, yaitu ketika saya ingin mencoba interaksi antar microservice dari app kelompok.
<br>

#### Reflection Publisher-3
