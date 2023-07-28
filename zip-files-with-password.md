# Creating encrypted ZIP files with password in PHP

You need at least PHP 7.2 to encrypt ZIP files with a password.

```php
<?php

$zip = new ZipArchive();

$zipFile = __DIR__ . '/output.zip';
if (file_exists($zipFile)) {
    unlink($zipFile);
}

$zipStatus = $zip->open($zipFile, ZipArchive::CREATE);
if ($zipStatus !== true) {
    throw new RuntimeException(sprintf('Failed to create zip archive. (Status code: %s)', $zipStatus));
}

$password = 'top-secret';
if (!$zip->setPassword($password)) {
    throw new RuntimeException('Set password failed');
}

// compress file
$fileName = __DIR__ . '/test.pdf';
$baseName = basename($fileName);
if (!$zip->addFile($fileName, $baseName)) {
    throw new RuntimeException(sprintf('Add file failed: %s', $fileName));
}

// encrypt the file with AES-256
if (!$zip->setEncryptionName($baseName, ZipArchive::EM_AES_256)) {
    throw new RuntimeException(sprintf('Set encryption failed: %s', $baseName));
}

$zip->close();
```
# Extract file with set password

```php
<?php
    // Đường dẫn và tên file zip muốn giải nén
    $zipFilePath = storage_path('OQSsiquc01req_1690527081EXv2XqrP8w.zip');

    // Đường dẫn đến thư mục muốn giải nén file zip
    $extractToPath = '/app/storage/app/';

    // Mật khẩu của file zip
    $password = '123456';

    // Tạo đối tượng ZipArchive
    $zip = new ZipArchive();

    // Mở file zip để giải nén
    if ($zip->open($zipFilePath) === true) {
        // Kiểm tra mật khẩu của file zip (thông qua phương thức setPassword khi giải nén)
        $zip->setPassword($password);
        // Giải nén toàn bộ file trong file zip vào thư mục chỉ định
        $zip->extractTo($extractToPath);

        // Đóng file zip sau khi hoàn thành giải nén
        $zip->close();

        echo "File đã được giải nén với mật khẩu thành công.";
    } else {
        echo "Không thể mở file zip.";
    }
```
