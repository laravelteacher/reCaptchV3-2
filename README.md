                    How to make a reCaptch V3 in Laravel App with validate

step 1 - if we wanna this project in Register, we need a make new RegisterController so writeh in Terminal

  php artisan make:controller registerController
  
  
  
  
  

step 2 - put the below new code in registerController


    public function register(Request $request)
{
    $vars = array(
        'secret' => env('G_RECAPTCHA_SECRET_KEY'),
        "response" => $request->input('recaptcha_v3')
    );
    $url = "https://www.google.com/recaptcha/api/siteverify";
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $vars);
    $encoded_response = curl_exec($ch);
    $response = json_decode($encoded_response, true);
    curl_close($ch);
    if($response['success'] && $response['action'] == 'homepage' && $response['score']>0.5) {
    //     if($this->validate($request, [
    //         'password' => ['required', 'string', 'min:8', 'confirmed']
    //     ])){
    //     User::create([
    //         'name' => $request['name'],
    //         'email' => $request['email'],
    //         'password' => Hash::make($request['password']),
    //     ]);
    //     return redirect('login');
    // }else{
    //     return redirect()->back()->withErrors(['password' => 'The Password isnt match']);
    // }
    } else {
        // return redirect()->back()->withErrors(['recaptcha_v3' => 'The reCaptcha_v3 is required']);
    }
}






step 3 - add the below code in register.blade.php


           
            
            @error('recaptcha_v3')
            <div class="alert alert-danger" role="alert">
               <p>{{ $message }}</p>
            </div>   
            @enderror
            
            
            @error('password')
            <div class="alert alert-primary" role="alert">
               <p>{{ $message }}</p>
            </div>   
            @enderror
            

                

                       
                        <input type="hidden" name="recaptcha_v3" id="recaptcha_v3">
                        
    <script src="https://www.google.com/recaptcha/api.js?render={{env('G_RECAPTCHA_SITE_KEY')}}"></script>
<script>
    grecaptcha.ready(function() {
        grecaptcha.execute("{{env('G_RECAPTCHA_SITE_KEY')}}", {action: 'homepage'}).then(function(token) {
            if(token) {
                //js
                document.getElementById('recaptcha_v3').value = token;
                //if you use jquery library
                $("#recaptcha_v3").val(token);
            }
        });
    });
</script>





step 4 - get new code reCaptcha from google  in this address

https://www.google.com/recaptcha/admin/create

step 5 - put the below code in .env

G_RECAPTCHA_SITE_KEY="please add your site key here"
G_RECAPTCHA_SECRET_KEY="please add your secret key here"

step 6 - put route in web.php

Route::post('/submit','registerController@register');




it work verry good

i test it for code of reCaptcha if the code incorrect show alert 
i test it again

now i make the code 

dont forget to subscribe

thanks 

  
