# Sửa lỗi trang web có PHP
Các sinh viên thực hiện:
1. Bùi Công Định - 24520303
2. Phạm Ngọc Dũng - 24520346
3. Phan Hoàng Dũng - 24520348
4. Huỳnh Đăng Khoa - 24520815

Repo đã sửa lỗi:

## Lỗi cú pháp
1. Sai ngoặc trong tệp pages/customer.php:
    ```php
    foreach ($customers as $customer) {
        if ($customer['active') {
            $activeCustomers[] = $customer;
        }
    }
    ```
    Sửa thành:
    ```php
    foreach ($customers as $customer) {
        if ($customer['active']) {
            $activeCustomers[] = $customer;
        }
    }
    ```
2. Thiếu dấu chấm phẩy (;) trong pages/reports.php:
    ```php
    $reportRows = []
    ```
    Sửa thành:
    ```php
    $reportRows = [];
    ```
3. Thiếu đóng ngoặc vuông trong pages/settings.php:
    ```php
    $config = [
    'currency' => 'USD',
    'timezone' => 'Asia/Ho_Chi_Minh',
    'language' => 'en',
    ;
    ?>
    ```
    Sửa thành:
    ```php
    $config = [
    'currency' => 'USD',
    'timezone' => 'Asia/Ho_Chi_Minh',
    'language' => 'en',
    ];
    ?>
    ```
## Lỗi logic
1.  Lấy loại sản phẩm sai trong pages/reports.php:
    ```php
    $category = $product['name'];
    ```
    Thay vì lấy loại của sản phẩm, code lại lấy tên sản phẩm gán vào `category`. Sửa thành:
    ```php
    $category = $product['category'];
    ```
2.  Trong includes/data.php:
    a. Hàm `calculate_order_total` tính tổng các mặt hàng nhưng lại không nhân số lượng mặt hàng:
    ```php
            $subtotal += $products[$item['sku']]['price'];
    ```
    Thành:
    ```php
            $subtotal += $products[$item['sku']]['price'] * $item['qty'];
    ```
    b. Hàm  `calculate_inventory_value` tính giá trị hàng trong kho nhưng lại cộng giá trị hàng với số lượng:
    ```php
            $value += $product['price'] + $product['stock'];
    ```
    Sửa thành:
    ```php
            $value += $product['price'] * $product['stock'];
    ```
3.  Trong file pages/dashboard.php:
    a. Logic tính `completedOrders` và `totalRevenue` lại lấy hàng `pending`:

    ```php
    if ($order['status'] === 'pending') {
        $completedOrders++;
        $totalRevenue += calculate_order_total($order, $products);
    }
    ```
    Sửa thành:
    ```php
    if ($order['status'] === 'completed') {
        $completedOrders++;
        $totalRevenue += calculate_order_total($order, $products);
    }
    ```
4.  Trong file pages/order.php:
    Logic lọc đơn hàng đang chờ nhưng lọc `completed`:

    ```php
    foreach ($orders as $order) {
        if ($order['status'] === 'completed') {
            $pendingOnly[] = $order;
        }
    }
    ```

    Sửa thành:

    ```php
    foreach ($orders as $order) {
        if ($order['status'] === 'pending') {
            $pendingOnly[] = $order;
        }
    }
    ```
5.  Trong file pages/checkout.php:
    a. Tổng `subtotal` bị ghi đè mỗi lần lặp:
    ```php
    foreach ($cart as $item) {
        $subtotal = $products[$item['sku']]['price'] * $item['qty'];
    }
    ```
    Sửa thành:
    ```php
    foreach ($cart as $item) {
        $subtotal += $products[$item['sku']]['price'] * $item['qty'];
    }
    ```
    b. Thuế VAT tính trước khi giảm giá thay vì sau khi giảm giá:
    ```php
    $vat = $subtotal * 0.1;
    ```
    Sửa thành:
    ```php
    $vat = ($subtotal - $discountValue) * 0.1;
    ```
