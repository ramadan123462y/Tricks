<?php

namespace App\Http\Services;

use GuzzleHttp\Client;


class  Http
{
    protected $client;

    public function __construct(Client $client)
    {
        $this->client = $client;
    }

    public function send_request($url, $method, $headers, $body = [])
    {

        $request = new \GuzzleHttp\Psr7\Request($method, $url, $headers);
        if (!$body) {

            return false;
        }


        $response =$this->client->send($request, [
            'json' => $body
        ]);


        if ($response->getStatusCode() != 200) {

            return false;
        }


        // تحويل الاستجابة إلى تنسيق JSON
        $responseData = json_decode($response->getBody(), true);
        return $responseData;

        // الحصول على قيمة InvoiceURL
        // $invoiceURL = $responseData['Data']['InvoiceURL'];

        // return redirect($invoiceURL);

    }
}
