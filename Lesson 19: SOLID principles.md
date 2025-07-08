<details>
  <summary><strong> SOLID principles </strong></summary>

**SOLID** là tập hợp **5 nguyên tắc thiết kế** phần mềm giúp tạo ra mã nguồn **dễ bảo trì, mở rộng và linh hoạt** hơn.

![image](https://github.com/user-attachments/assets/68719767-013e-4eb8-982c-0744c429c2c7)


<details>
  <summary><strong> Nguyên tắc đơn trách nhiệm </strong></summary>

**S - Single Responsibility Principle (SRP) - Nguyên tắc đơn trách nhiệm**: **Mỗi phần** của chương trình chỉ nên làm **một việc duy nhất**. Nếu một phần có nhiều nhiệm vụ, nó sẽ khó bảo trì và dễ gây lỗi khi thay đổi. Nguyên tắc này giúp mã nguồn dễ bảo trì, dễ mở rộng và dễ kiểm thử hơn.

Ta có chương trình sau:

```cpp
class Sensor
{
    public:
        // Xử lý dữ liệu
        void processData(){}
       
        // Lưu trữ dữ liệu
        void saveData(){}

        // Gửi dữ liệu đi
        void sendData(){}
};
```

Bản thân class `Sensor` trên đảm nhận tận **ba việc khác nhau** là xử lý, lưu trữ và gửi. Điều này là vi phạm **SRP** do đó ta cần phải sửa lại cho phù hợp.

```cpp
// Class xử lý dữ liệu
class Process
{
    public:
        void processData(){}
};

// Class lưu trữ dữ liệu
class Save
{
    public:
        void saveData(){}
};

// Class gửi dữ liệu
class Send
{
    public:
        void sendData(){}
};
```

Như trên đối với các **công việc khác nhau** thì ta đều đã chia ra làm từng **class riêng biệt** và đảm bảo được **SRP**.

</details>



<details>
  <summary><strong> Nguyên tắc đóng mở </strong></summary>

**O - Open/Closed Principle (OCP) - Nguyên tắc đóng mở**: Cho phép **mở rộng** nhưng **không sửa đổi** mã cũ. Khi cần thêm tính năng mới, hãy thêm mã mới thay vì chỉnh sửa mã hiện có. Tránh sửa đổi mã nguồn cũ, giúp phần mềm ổn định hơn.

Ví dụ:
```cpp
class SpeedAlert
{
    public:
        void sendAlert(double speed)
        {
            if (speed > 100)
            {
                cout << "Cảnh báo: Vượt quá tốc độ!" << endl;
            }
        }
};
```

Ở đây ta có một hệ thống cảnh báo tốc độ, ban đầu hệ thống này chỉ hỗ trợ cảnh báo bằng âm thanh. Nếu ta muốn thêm cảnh báo bằng đèn LED, ta phải sửa code trong `sendAlert()`, vi phạm OCP. Thay vào đó ta viết như sau:

```cpp
#include <iostream>
using namespace std;

class Device_Alert{
    public:
        virtual void sendAlert(double speed) = 0;
};
class Sound_Alert : public Device_Alert{
    public:
        void sendAlert(double speed) override{
            if (speed > 100){
                cout << "Cảnh báo: Vượt quá tốc độ! (Âm thanh)" << endl;
            }
        }
};
class LED_Alert : public Device_Alert{
    public:
        void sendAlert(double speed) override{
            if (speed > 100){
                cout << "Cảnh báo: Vượt quá tốc độ! (Đèn LED)" << endl;
            }
        }
};
   
int main(){
    Sound_Alert sound;
    LED_Alert led;

    sound.sendAlert(110);
    led.sendAlert(180);

    return 0;
}
```

Ở đây ta:
+  Tạo một class cha chứa hàm `sendAlert()`. Đây sẽ là hàm cảnh báo chung cho các loại cảnh báo khác nhau và là hàm thuần ảo để các hàm con khác có thể kế thừa.
+  Đối với các cảnh báo khác nhau thì ta lại tạo các `class` khác nhau như `Sound_Alert` và `LED_Alert`, các hàm này đều kế thừa từ lớp cha `Device_Alert`.
+  Nếu ta muốn thêm một dạng cảnh báo khác, thì ta có thể thêm một lớp mới và để nó kế thừa `Device_Alert` để sử dụng chung phương thức `sendAlert()`.

</details>



<details>
  <summary><strong> Nguyên tắc thay thế Liskov </strong></summary>

**L - Liskov Substitution Principle (LSP) - Nguyên tắc thay thế Liskov**: Một lớp con có thể thay thế lớp cha mà không làm thay đổi tính đúng đắn của chương trình. Đảm bảo tính kế thừa không phá vỡ hành vi của hệ thống.

```cpp
class Rectangle{
    private:
        int width, height;

    public:
        virtual void setWidth(int w){
            width  = w;
        }

        virtual void setHeight(int h){
            height = h;
        }
};

class Square : public Rectangle{
    public:
        void setWidth(int w)  override{
            width = height = w;
        }

        void setHeight(int h) override{
            width = height = h;
        }
};
```

Ở đây ta có `Square` là lớp con của `Rectangle`, tuy nhiên ở lớp `Square` các phương thức `setWidth(int w)` và `setHeight(int h)` lại gán cho `width = height`. Điều này làm thay đổi đi tính đúng đắn của class cha vi phạm **LSP** vì `width` và `height` phải khác nhau. Do đó ta cần phải sửa lại khác:

```cpp
class Shape{
    public:
        virtual int getArea() = 0;
};
   
class Rectangle : public Shape {
    private:
        int width, height;

    public:
        void setDimensions(int w, int h){
            width = w; height = h;
        }

        int getArea() override{
            return width * height;
        }
};

class Square : public Shape {
    private:
        int side;

    public:
        void setSide(int s){
            side = s;
        }
       
        int getArea() override{
            return side * side;
        }
};
```

Bằng cách để cả `Square` và `Rectangle` cũng thừa kế `Shape` và có cho mình các thuộc tính riêng sẽ không làm ảnh hưởng đến tính đúng đắn của chương trình. Nếu sau lại cần thêm các hình dạng khác thì lại cho thừa kế chung và override lại phương thức.

</details>




<details>
  <summary><strong> Nguyên tắc phân tách giao diện </strong></summary>

**I - Interface Segregation Principle (ISP) - Nguyên tắc phân chia interface**: Một class không nên bị ép buộc triển khai những phương thức mà nó không sử dụng. Tránh việc các class con phải cài đặt các phương thức không liên quan.

```cpp
class IVehicle
{
    public:
        virtual void drive() = 0;       // phương tiện có thể lái
        virtual void refuel() = 0;      // tiếp nhiên liệu
        virtual void loadCargo() = 0;   // chở hàng hóa
};

class Bike : public IVehicle
{
    public:
        void drive() override
        {
            cout << "Lái xe đạp" << endl;
        }

        void refuel() override {}       // Không hợp lý với xe đạp
        void loadCargo() override {}     
```

Giao diện `IVehicle` có ba hàm thuần ảo, class `Bike` kế thừa từ giao diện phải override cả ba hàm thuần ảo mặc dù `Bike` không thể sử dụng phương thức `refuel()`. Như vậy chương trình trên đã vi phạm **ISP** vì ép buộc lớp `Bike` triển khai những phương thức mà nó không sử dụng. Để sửa chữa việc này ta phân tách như sau.

```cpp
// Interface cho phương tiện có thể lái
class IDriveable{
    public:
        virtual void drive() = 0;
};
   
// Interface cho phương tiện có thể lái
class IFuelable{
    public:
        virtual void refuel() = 0;
};

// Interface cho phương tiện có thể lái
class ICargo{
    public:
        virtual void loadCargo() = 0;
};

// Xe ô tô
class Car : public IDriveable, public IFuelable, public ICargo{
    public:
        void drive() override{ cout << "Lái ô tô" << endl; }
    
        void refuel() override{ cout << "Đổ xăng" << endl; }
    
        void loadCargo() override{ cout << "Chở hàng" << endl; }
};

// xe đạp
class Bike : public IDriveable, public ICargo{
    public:
        void drive() override{ cout << "Lái xe đạp" << endl; }

	      void loadCargo() override{ cout << "Chở hàng" << endl; }
};
```

Bằng cách chia các tính năng thành các giao diện khác nhau, như vậy đối với các lớp phương tiện khác nhau ta có thể chọn tính cho phù hợp. Đối với `Car` có thể kế thừa cả ba lớp `IDriveable`, `IFuelable` và `ICargo` trong khi lớp `Bike` chỉ cần kế thừa 2 lớp thôi tránh được việc triển khai phương thức mà nó không sử dụng.

</details>



<details>
  <summary><strong> Nguyên tắc đảo ngược phụ thuộc </strong></summary>

**D - Dependency Inversion Principle (DIP) - Nguyên tắc đảo ngược sự phụ thuộc**: Các phần quan trọng trong chương trình không nên dựa trực tiếp vào chi tiết cụ thể mà nên dựa vào** một giao diện chung**. Điều này giúp phần mềm linh hoạt hơn, dễ mở rộng và thay đổi mà không làm ảnh hưởng đến các phần khác.

Ví dụ: thay vì một công tắc chỉ bật được đèn, ta làm nó điều khiển bất kỳ thiết bị nào bằng cách dùng một giao diện chung.

```cpp
class LightBulb
{
    public:
        void turnOn() { /* Bật đèn */ }
};

class Switch
{
    private:
        LightBulb bulb;
       
    public:
        void operate() { bulb.turnOn(); }
};    
```

Ta có lớp `Switch` bị phụ thuộc vào lớp `LightBulb`, hàm `turnOn()` đã được viết sẵn việc thay đổi hàm có thể ảnh hưởng đến tính đúng đắn của lớp `LightBulb`. `Switch` bị phụ thuộc vào `LightBulb` thay vì một giao diện chung như vậy là đã vi phạm **DIP**.

```cpp
class Device{
    public:
        virtual void turnOn() = 0;
};
class LightBulb : public Device{
    public:
        void turnOn() override { /* Bật đèn */ }
};
class Fan : public Device{
    public:
        void turnOn() override { /* Bật quạt */ }
};
class Switch{
    private:
        Device *device;

    public:
        Switch(Device *d) : device(d){}
        void operate() { device->turnOn(); }
};
```

Ở đây cả ba lớp `LightBulb`, `Fan` và `Switch` đều kế thừa từ lớp `Device` (một giao diện chung cho cả 3). Như vậy ta không chỉ sử dụng lớp `Switch` cho lớp `LightBulb` mà còn cả lớp `Fan` và thậm chí cho các lớp khác nếu ta cần mở rộng thêm.

</details>





</details>
