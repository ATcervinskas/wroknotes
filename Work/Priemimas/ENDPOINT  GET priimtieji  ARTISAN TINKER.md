```php
use Illuminate\Http\Request;
use App\Api\v1\Services\Lama;

// Create a request with a fake URI and the actual POST data
$request = Request::create('api/v1/lama', 'POST', [
    'method' => 'GET',
    'request' => 'etapai/1/kvieciamieji/166127/isldok/regisrasas',
    'signature' => null, 
]);

$lama = new Lama($request);

$response = $lama->handle();

print_r($response);

```