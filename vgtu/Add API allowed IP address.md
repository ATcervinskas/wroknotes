
Using tinker

use App\Models\ApiUser;
use App\Models\ApiUserIpAddress;

//create user
ApiUser::create(['name'=>'GUEST_USER', 'comments'=>'Unauthorized user']);

//Find user with a specific ID and assign an IP address to them."
$user = ApiUser::find(1)
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '158.129.192.8','comment' => 'Oracle 9SP',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '158.129.192.165','comment' => 'Oracle 12 PRD',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '10.21.10.32','comment' => 'Ona',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '10.21.10.35','comment' => 'Arturas',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '10.21.10.34','comment' => 'Dmitrij',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '10.21.16.125','comment' => 'Dmitrij',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '10.21.54.3','comment' => 'Dmitrij',]));
$user = ApiUser::find(2)
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '158.129.159.242','comment' => 'VU',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '158.129.159.161','comment' => 'VU',]));
$user = ApiUser::find(4)
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '13.438.93.19','comment' => 'Dokobit',]));
$user->ipAddress()->save(new ApiUserIpAddress(['ip_address' => '13.48.93.139','comment' => 'Dokobit',]));

$user->save();

