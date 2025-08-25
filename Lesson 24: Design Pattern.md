<details>
  <summary><strong> Design Pattern </strong></summary>




<details>
  <summary><strong> Singleton </strong></summary>

**Singleton** là một mẫu thiết kế thuộc nhóm Creational (mẫu khởi tạo), nó đảm bảo rằng một class chỉ có **một đối tượng duy nhất** được tạo ra, và cung cấp **một phương thức** để truy cập đến đối tượng đó từ bất kỳ đâu trong chương trình.

**Singleton** thường sử dụng cho những hệ thống chỉ cần một phiên bản duy nhất như: kết nối cơ sở dữ liệu, bộ nhớ đệm (cache), logger để ghi log, hoặc cấu hình hệ thống.

Các thành phần chính của Singleton:

🔍 **Private Constructor:**
+  Đảm bảo rằng không ai có thể khởi tạo đối tượng từ bên ngoài class.

🔍 **Static Instance:** 
+  Đối tượng tĩnh duy nhất của class.
+  Không thể tạo ra nhiều hơn một đối tượng của class Singleton.

🔍 **Static Method:** 
+  Phương thức để truy cập đến đối tượng duy nhất từ mọi nơi trong chương trình.

```cpp
void gpioInit();
void gpioSetPin(int pin, bool value);
void gpioReadPin(int pin);

class GpioManager{
    private:
        GpioManager(){ gpioInit(); }
        static GpioManager* instance;

    public:
        static GpioManager *getInstace(){
            if (!instance){
                instance = new GpioManager(); // 0xc8
            }
            return instance;
        }
        void setPin(int pin, bool value){ gpioSetPin(pin, value); }
        void readPin(int pin){ gpioReadPin(pin); }
};
```
Trong đó:
+  `GpioManager(){gpioInit();}`: Private Constructor đảm bảo rằng các hàm khởi tạo đối tượng sẽ không thể thực hiện bên ngoài.
+  `static GpioManager* instance`: Đây là đối tượng tĩnh duy nhất của class, đảm nhiệm quản lý đối tượng được tạo.
+  ```cpp
        static GpioManager *getInstace(){
            if (!instance){
                instance = new GpioManager(); // 0xc8
            }
            return instance;
        }
   ```
Phương thức để truy cập đến đối tượng duy nhất từ mọi nơi trong chương trình đồng thời đảm bảo rằng không thể tạo ra nhiều hơn một đối tượng trong class Singleton.

Triển khai trong `main`:
```cpp
// 0xc8 : địa chỉ cố định
GpioManager* GpioManager::instance = nullptr; 

int main(){
    GpioManager* gpioManager = GpioManager::getInstace();
    gpioManager->setPin();
    gpioManager->readPin();
    GpioManager* gpioManager2 = GpioManager::getInstace();
    return 0;
}
```

</details>




<details>
  <summary><strong> Observer </strong></summary>

**Observer** là một mẫu thiết kế thuộc nhóm **Behavioral** (mẫu hành vi), nó định nghĩa một mối quan hệ phụ thuộc **one-to-many** giữa các đối tượng, nghĩa là khi một **đối tượng thay đổi trạng thái** (Subject), tất cả các **đối tượng phụ thuộc** (Observers) vào nó sẽ được **tự động thông báo và cập nhật**.

<img width="1041" height="514" alt="image" src="https://github.com/user-attachments/assets/6f8afb23-9916-43ad-a581-41f3e1eda0d2" />

Các thành phần chính:

🔍 **Subject**:
+  Đối tượng giữ trạng thái và chịu trách nhiệm thông báo cho các Observer về sự thay đổi.
+  Cung cấp phương thức để thêm, xóa, và thông báo Observer.

🔍 **Observer**:
+  Một interface định nghĩa phương thức cập nhật dữ liệu.
+  Observer sẽ thực hiện hành động khi nhận được thông báo từ Subject.

🔍 **Concrete Observer**:
+  Là các class kế thừa từ Observer và thực hiện phương thức cập nhật dữ liệu.
+  Các class này sẽ nhận thông báo từ Subject và xử lý thông tin.

🔍 **Concrete Subject**:
+  Là class triển khai cụ thể của Subject.
+  Chứa logic để quản lý danh sách Observer và trạng thái của Subject.

Sơ đồ class của Observer:

<img width="899" height="535" alt="image" src="https://github.com/user-attachments/assets/dfb4d47c-8c53-4ea1-8a0e-cd78848c0eda" />

**Đặc điểm chính của Observer Pattern:**

🔍 **Mối quan hệ giữa Subject và Observer:**
+  Subject giữ một danh sách các Observer. 
+  Các Observer đăng ký nhận thông báo từ Subject khi có sự thay đổi trạng thái. 
+  Observer có thể thêm, xóa hoặc cập nhật trong danh sách này.

🔍 **Tự động thông báo (Push Notification):**
+  Khi trạng thái của Subject thay đổi, nó sẽ tự động thông báo cho tất cả các Observer đã đăng ký. 
+  Các Observer không cần chủ động kiểm tra trạng thái của Subject mà sẽ nhận thông báo ngay khi có thay đổi.

🔍 **Tính linh hoạt và mở rộng:**
+  Observer Pattern cho phép dễ dàng thêm hoặc xóa các Observer mà không cần thay đổi Subject.
+  Observer có thể dễ dàng ngừng nhận thông báo từ Subject bằng cách hủy đăng ký, giúp kiểm soát tốt hơn việc quản lý tài nguyên và sự kiện trong hệ thống.

🔍 **Nhiều Observer có thể theo dõi một hoặc nhiều Subject:**
+  Nhiều Observer có thể cùng theo dõi một Subject. Điều này cho phép cùng một sự kiện trong Subject có thể ảnh hưởng đến nhiều đối tượng khác nhau.
+  Một Observer có thể đăng ký để nhận thông báo từ nhiều Subject khác nhau, và mỗi Subject sẽ thông báo cho Observer khi có sự thay đổi liên quan.

Ví dụ minh họa cho Observer:
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
using namespace std;

// Interface for observers (display, logger, etc.)
class Observer {
public:
    virtual void update(float temperature, float humidity, float light) = 0;
};

// Subject (SensorManager) holds the state and notifies observers
class SensorManager {
    float temperature;
    float humidity;
    float light;
    vector<Observer*> observers;

public:
    void registerObserver(Observer* observer) {
        observers.push_back(observer);
    }

    void removeObserver(Observer* observer) {
        observers.erase(remove(observers.begin(), observers.end(), observer), observers.end());
    }

    void notifyObservers() {
        for (auto observer : observers) {
            observer->update(temperature, humidity, light);
        }
    }

    void setMeasurements(float temp, float hum, float lightLvl) {
        temperature = temp;
        humidity = hum;
        light = lightLvl;
        notifyObservers();
    }
};

// Display component (an observer)
class Display : public Observer {
public:
    void update(float temperature, float humidity, float light) override {
       cout << "Display: Temperature: " << temperature
            << ", Humidity: " << humidity
            << ", Light: " << light << endl;
    }
};

// Logger component (an observer)
class Logger : public Observer {
public:
    void update(float temperature, float humidity, float light) override {
        cout << "Logging data... Temp: " << temperature
             << ", Humidity: " << humidity
             << ", Light: " << light << endl;
    }
};
```
Phân tích thành phần:

🌟 **Observer**: Interface dùng cho việc cập nhập nhiệt độ, độ ẩm, ánh sáng
```cpp
class Observer {
public:
    virtual void update(float temperature, float humidity, float light) = 0;
};
```

🌟 **Concrete Subject:**: Giữ các trạng thái của cảm biến như `temperature`, `humidity`, `light` và các phương thức để truy cập và thông báo cho các Observer về sự thay đổi. Chứa logic để quản lý danh sách Observer và trạng thái của Subject.
```cpp
class SensorManager {
    float temperature;
    float humidity;
    float light;
    vector<Observer*> observers;

public:
    void registerObserver(Observer* observer) {
        observers.push_back(observer);
    }

    void removeObserver(Observer* observer) {
        observers.erase(remove(observers.begin(), observers.end(), observer), observers.end());
    }

    void notifyObservers() {
        for (auto observer : observers) {
            observer->update(temperature, humidity, light);
        }
    }

    void setMeasurements(float temp, float hum, float lightLvl) {
        temperature = temp;
        humidity = hum;
        light = lightLvl;
        notifyObservers();
    }
};
```
Trong đó:

🔍 `vector<Observer*> observers;`: là danh sách (mảng động) các con trỏ của các Observer khác nhau mà cùng đăng kí vào 1 Subject.

🔍 `void registerObserver(Observer* observer)`: Phương thức dùng để đăng kí cho các Observer để theo dõi một Subject, các Observer dược đăng kí sẽ được lưu vào mảng vector `observers`.
```cpp
   void registerObserver(Observer* observer) {
        observers.push_back(observer);
    }
```

🔍 `void removeObserver(Observer* observer)`: Phương thức dùng để xóa đăng kí của một Observer mong muốn ra khỏi mảng quản lý đăng kí:
```cpp
    void removeObserver(Observer* observer) {
      observers.erase(remove(observers.begin(), observers.end(), observer), observers.end());
  }
  ```
+ `remove(observers.begin(), observers.end(), observer)`: Dùng thuật toán `remove` để "dồn" tất cả phần tử khác `observer` về đầu vector, trả về iterator trỏ đến vị trí mới của `end()` (ngay sau phần tử cuối hợp lệ).
+ `erase(remove(observers.begin(), observers.end(), observer), observers.end())`: Xóa đi các phần tử giữa iterator trỏ đến vị trí mới và vị trí cuối của mảng `observers`. Khi này con trỏ `observer` mong muốn sẽ bị xóa đi (dữ liệu được trỏ đến không bị ảnh hưởng)

🔍 `void notifyObservers()`: Gửi thông báo cho toàn bộ các Observer

🌟 **Concrete Observer**: Là các class kế thừa từ Observer và thực hiện phương thức cập nhật dữ liệu. Các class này sẽ nhận thông báo từ Subject và xử lý thông tin.
```cpp
// Display component (an observer)
class Display : public Observer {
public:
    void update(float temperature, float humidity, float light) override {
       cout << "Display: Temperature: " << temperature
            << ", Humidity: " << humidity
            << ", Light: " << light << endl;
    }
};

// Logger component (an observer)
class Logger : public Observer {
public:
    void update(float temperature, float humidity, float light) override {
        cout << "Logging data... Temp: " << temperature
             << ", Humidity: " << humidity
             << ", Light: " << light << endl;
    }
};
```

Triển khai trong main:

```cpp
int main() {
    SensorManager sensorManager;

    Display display;
    Logger logger;

    sensorManager.registerObserver(&display);
    sensorManager.registerObserver(&logger);

    sensorManager.setMeasurements(25.0, 60.0, 700.0); // Simulate sensor data update
    sensorManager.setMeasurements(26.0, 65.0, 800.0); // Another sensor update

    return 0;
}
```

+  Khởi tạo hai Observer `display` và `logger`.
+  Đăng kí cho hai Observer trên qua phương thức `registerObserver()`.
+  Mỗi khi thay đổi dữ liệu bằng phương thức `setMeasurements` thì cả 2 Observer trên sẽ đều được thông báo.

Kết quả:
```
Display: Temperature: 25, Humidity: 60, Light: 700
Logging data... Temp: 25, Humidity: 60, Light: 700
Display: Temperature: 26, Humidity: 65, Light: 800
Logging data... Temp: 26, Humidity: 65, Light: 800
```


</details>







</details>
