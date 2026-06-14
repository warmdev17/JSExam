# Bài Kiểm Tra Lập Trình - Tuần 1

## Câu 1

- **Yêu cầu:** Nhận định "Đoạn code này sẽ in ra lần lượt các số 0, 1, 2" là đúng hay sai? Giải thích và sửa lỗi nếu có.
- **Trả lời:** Nhận định trên là **đúng**.
- **Giải thích nguyên lý:** Khi khai báo biến lặp bằng từ khóa `let`, JavaScript sẽ tạo ra một block scope (vùng nhớ độc lập) cho mỗi vòng lặp. Nhờ cơ chế closure, mỗi hàm callback bên trong `setTimeout` sẽ tham chiếu đến đúng giá trị của `i` tại thời điểm vòng lặp đó thực thi. Do đó, kết quả in ra sẽ là 0, 1, 2.
  Ngược lại, nếu sử dụng `var`, biến `i` sẽ dùng chung một vùng nhớ (function/global scope) trong suốt quá trình lặp. Khi vòng lặp kết thúc (`i = 3`), các `setTimeout` mới bắt đầu chạy và tất cả đều tham chiếu đến giá trị cuối cùng này, dẫn đến việc in ra 3, 3, 3.

---

## Câu 2

- **Yêu cầu:** Nhận định "Chỉ cần dùng duy nhất hàm .map() kết hợp với câu lệnh if là có thể trả về một mảng nguyên sạch chỉ chứa id..." là đúng hay sai? Viết code giải quyết bài toán bằng Arrow Function.
- **Trả lời:** Nhận định trên là **sai**.
- **Giải thích:** Đặc điểm của phương thức `.map()` là luôn trả về một mảng mới có độ dài bằng chính xác độ dài của mảng ban đầu. Nếu chỉ dùng `.map()` kèm `if`, các object không thỏa mãn điều kiện sẽ trả về giá trị `undefined` trong mảng kết quả chứ không bị loại bỏ hoàn toàn.
- **Code thực hành:**

```javascript
// Cách 1: Kết hợp filter và map (Dễ đọc)
const adminIds = users
  .filter((u) => u.role === "admin" && u.active)
  .map((u) => u.id);

// Cách 2: Sử dụng reduce (Tối ưu vòng lặp)
// const adminIds = users.reduce((acc, u) => {
//   if (u.role === 'admin' && u.active) acc.push(u.id);
//   return acc;
// }, []);
```

---

## Câu 3

- **Yêu cầu:** Nhận định "Cú pháp Spread Operator (...) khi dùng để sao chép một object sẽ luôn tạo ra một bản sao độc lập hoàn toàn (deep copy)..." là đúng hay sai?
- **Trả lời:** Nhận định trên là **sai**.
- **Giải thích:** Cú pháp Spread Operator chỉ thực hiện sao chép nông (shallow copy). Đối với các thuộc tính ở cấp độ đầu tiên (top-level), nó tạo ra bản sao mới. Tuy nhiên, đối với các thuộc tính dạng tham chiếu lồng nhau (nested objects/arrays), nó vẫn sao chép địa chỉ vùng nhớ. Việc thay đổi dữ liệu bên trong mảng/object lồng sẽ làm biến đổi (mutate) cả object gốc.
- **Code thực hành:**

```javascript
const state = { user: "Admin", privileges: ["read", "write"] };

// Spread cả object bên ngoài và mảng bên trong để tránh mutate dữ liệu gốc
const newState = {
  ...state,
  privileges: [...state.privileges, "delete"],
};
```

---

## Câu 5

- **Yêu cầu 1:** Nhận định về tham số mặc định khi truyền `null` vào hàm `createOrderMessage`.
- **Trả lời:** Nhận định "tham số status sẽ tự động nhận giá trị mặc định là 'pending' do null..." là **sai**.
- **Giải thích:** Theo chuẩn ES6, tham số mặc định (default parameters) chỉ được kích hoạt khi đối số truyền vào có giá trị là `undefined` hoặc khi đối số bị bỏ trống hoàn toàn. Giá trị `null` được JavaScript xem là một giá trị có chủ đích hợp lệ, do đó tham số `status` sẽ nhận trực tiếp giá trị `null`.

- **Yêu cầu 2:** Viết logic thân hàm `createOrderMessage` dùng Template Literals.
- **Code thực hành:**

```javascript
function createOrderMessage(user, status = "pending", ...items) {
  if (items.length > 0) {
    return `Khách hàng ${user} có đơn hàng ${status} gồm các món: ${items.join(", ")}`;
  }

  return `Khách hàng ${user} có đơn hàng ${status} nhưng giỏ hàng trống.`;
}
```
