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








</details>
