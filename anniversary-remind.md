```php
<?php
//https://www.w3schools.com/php/phptryit.asp?filename=tryphp_intro
date_default_timezone_set('Asia/Ho_Chi_Minh');
$data = array(
    '11-10' => 'Sinh nhật bà nội',
    '09-10' => 'Sinh nhật tôi',
    '02-24' => 'Sinh nhật vợ',
    '08-19' => 'Sinh nhật cô Út',
    '07-09' => 'Sinh nhật Cọ',
    '08-16' => 'Sinh nhật Chít',
    '06-28' => 'Thôi nôi Cọ',
    '06-26' => 'Kỷ niệm ngày cưới',
    '07-11' => 'Đám giỗ ông bà cố nội',
    '06-20' => 'Đám giỗ ông bà nội của má',
    '07-07' => 'Đám giỗ chú Tâm',
    '06-13' => 'Sinh nhật em Mi',
    '05-10' => 'Sinh nhật anh vợ',
    
);

ksort($data);

function convertWeekdayToVietnamese($weekday) {

    switch ($weekday) {
        case "Monday":
            $weekdayConverted = 'Thứ Hai';
            break;
        case "Tuesday":
            $weekdayConverted = 'Thứ Ba';
            break;
        case "Wednesday":
            $weekdayConverted = 'Thứ Tư';
            break;
        case "Thursday":
            $weekdayConverted = 'Thứ Năm';
            break;
        case "Friday":
            $weekdayConverted = 'Thứ Sáu';
            break;
        case "Saturday":
            $weekdayConverted = 'Thứ Bảy';
            break;
        case "Sunday":
            $weekdayConverted = 'Chủ Nhật';
            break;
        default:
    }

    return $weekdayConverted;
}

foreach ($data as $key => $value) {
    $today = date('Y-m-d');
    $year = date('Y');
    $myDate = sprintf("%s-%s", $year, $key);

    if ($today < $myDate) {
        $weekday = date('l', strtotime($myDate));
        $weekday = convertWeekdayToVietnamese($weekday);
        $diff = abs(strtotime($myDate) - strtotime($today));
        $years = floor($diff / (365*60*60*24));
        $months = floor(($diff - $years * 365*60*60*24) / (30*60*60*24));
        $days = floor(($diff - $years * 365*60*60*24 - $months*30*60*60*24) / (60*60*24));
        $result = '';
        if ($months == 0) {
            $result = $value . " còn " . $days . " ngày ($weekday) <br>";
        } else {
            $result = $value . " còn " . $months . " tháng, " . $days . " ngày  ($weekday) <br>";
        }

        echo $result;
    }
}
```
