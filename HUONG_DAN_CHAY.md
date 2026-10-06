# UR3 LLM Control
## Yêu cầu tiên quyết

Đảm bảo bạn đã build workspace ROS 2:

```bash
cd ~/ros2_ws
colcon build --packages-select ur3_llm_control
```

---

## Hướng dẫn khởi chạy (Multi-terminal)

Mở 3 cửa sổ Terminal độc lập và thực hiện lần lượt theo thứ tự dưới đây:

### Terminal 0: Khởi động 9router & Lấy API Key
Khởi chạy dịch vụ 9router trên máy để tạo endpoint và lấy API key:

```bash
9router start
```
> Giữ terminal này hoạt động trong suốt quá trình chạy robot. Sao chép lại API_KEY được cấp để dùng cho Terminal 2.

### Terminal 1: Khởi chạy mô phỏng (Gazebo & RViz2)
Mở terminal mới để nạp mô hình robot và môi trường mô phỏng:

```bash
cd ~/ros2_ws
source install/setup.bash
ros2 launch ur3_llm_control sim.launch.py
```

* **Dành cho máy yếu / không cần GUI Gazebo:**
  ```bash
  ros2 launch ur3_llm_control sim.launch.py gazebo_gui:=false
  ```

> ⚠️ **Lưu ý:** Chờ cho đến khi terminal in dòng:
> ```text
> You can start planning now!
> ```
> và giao diện RViz đã hiển thị đầy đủ cánh tay robot trước khi sang Terminal 2.

###Terminal 2: Chạy LLM Robot Node
Mở terminal mới, cấu hình biến môi trường API key và kích hoạt node điều khiển:
```bash
cd ~/ros2_ws
source install/setup.bash

# Cấu hình API Key và Model
export NINEROUTER_API_KEY="sk-key_9router_cua_ban"
export NINEROUTER_MODEL="ag/gemini-3.8-flash-high"

# Khởi chạy node điều khiển
ros2 run ur3_llm_control llm_robot_node --ros-args -p use_sim_time:=true
```
## Recommended command
Command> Arrange all objects according to my student ID.
Command> Sắp xếp các khối theo mã số sinh viên của tôi.
Command> Put the green cube in zone C.          (C đang có khối khác -> dọn trước)
Command> Where are the blocks?                  (chỉ quan sát)
Command> state                                  (in trạng thái camera)
Command> Move the orange cube to zone A.        (không có khối cam -> TASK REJECTED)
Command> Rotate the shoulder joint to 90 degrees.   (lệnh khớp -> bị validator chặn)

## Xử lý lỗi thường gặp

* **Lỗi `Package 'ur3_llm_control' not found`:**
  Do terminal chưa nhận workspace overlay. Chạy `source install/setup.bash` từ thư mục gốc `~/ros2_ws`.
* **Robot không phản hồi lệnh từ node:**
  Kiểm tra lại xem cờ `-p use_sim_time:=true` đã được truyền vào khi chạy `llm_robot_node` hay chưa (bắt buộc khi chạy với Gazebo).
