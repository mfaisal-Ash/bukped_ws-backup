# Otorisasi Token

Pada bagian ini kita akan coba menggunakan login untuk otorisasi dari Frontend ke Backend. Disini kita menggunakan whatsauth sebagai otorisasi. https://github.com/whatsauth/wauthjs pertama kita deploy terlebih dahulu Frontend js dari whatsauth.

## WAuthJS : Web Socket Authentication using Vanilla JS and PASETO

Use this js for Two-Factor Authentication (2FA) WhatsApp Application Login. We choose PASETO over JWT because its security and simplicity.
This library using [englishextra/qrjs2](https://github.com/englishextra/qrjs2) to generate QR.

## How to use

1. Copy wauth.js to your asset public static folder. For example : assets/js folder.
2. add this div and p tag into your login section page in your login html page. For examle : inside login div/section before form

    ```html
    <div id="whatsauthqr"><div></div></div>
    <p id="whatsauthcounter">hi</p>
    ```

3. Check your login form html tag. Please add id in form, input, and button tag.
   For Example, we have form login :

   ```html
   <form action="#" method="post" id="loginform">
    <input type="text" name="id_siap" id="user_name">
    <input type="password" name="password" id="user_pass">
    <button type="submit" id="login">Sign In</button>
   </form>
   ```

   Edit const declaration in wauthjs.js according to id in login form.

   ```js
    const using_click = true;        // true = id_button.click()    |   false = id_form.submit()
    const id_user = 'user_name';     //id of username input text. For example : <input type="text" name="id_siap" id="user_name">
    const id_pass = 'user_pass';     //id of password input. For Example : <input type="password" name="password" id="user_pass">
    const id_form = 'loginform';     //id of form tag. For Example : <form action="#" method="post" id="loginform">
    const id_button = 'login';       //id of login button. For Example : <button type="submit" class="btn btn-primary btn-block" id="login">Sign In</button>

    const auth_ws = 'd3NzOi8vYXV0aC51bGJpLmFjLmlkL3dzL3doYXRzYXV0aC9xcg==';    //wss URL using btoa(). In this example : btoa("wss://auth.ulbi.ac.id/ws/whatsauth/qr");
    const keyword = 'aHR0cHM6Ly93YS5tZS82MjgxMTIwMDAyNzk/dGV4dD13aDR0NWF1dGgw';  //whatsapp API with prefix keyword using btoa(). In this example : btoa("https://wa.me/628112000279?text=wh4t5auth0");

    const interval = 30;     // qrcode change interval in second
    const maxqrwait = 90;    // maximum qrcode display/ time out in second, usually = 3 x interval.
   ```

   Add wauth.js script in the end your body login html page, for example : before </body> tag.

   ```html
   <script src="assets/js/wauth.js"></script>
   ```

4. Build your users database, consists userid with whatsapp phone number(with international format, eg:6281112233).
5. Build your whatsauth server or use existing, add your apps domain in your CORS Config.

## How it Works

In this section explain how wauth.js works, this section need if you want to be contributor. $ is a variable.

1. QR shows by Loop for $interval until $maxqrwait.
2. uuid generated by random string combine with UUID, use dot(.) for separator and several information about device.
   * prefix : d for desktop, m for mobile, and v4 if paseto.
   * suffic : url information of current access as Agent URL.
3. In mobile version, you might click QR(no need to scan) to login to generate magic link
4. Magic Link use PASETO token passing into agent URL as uuid parameter. For example of magic link : <https://Agent/URL/?uuid=PASETOKEN>
5. There is Cookies in paseto format with Login as name. Use it to authentication with backend

## Membuat backend whatsauth

Pada bagian backend kita siapkan database user, dan setting dari controller whatsauth yang sudah ada di boilerplate iteung.

Setiap controller yang sudah dibuat, tambahkan otorisasi dengan menangkap paseto pada header
```go
h := new(Header)
if err := c.ReqHeaderParser(h); err != nil {
    return err
}
phone, err := watoken.Decode(config.PublicKey, h.Login)
if err != nil {
    return err
}

```

## Kerjakan
* Implementasikan Whatsauth di repo masing masing pada bagian depan github pages masing masing repo (30)
* Setiap frontend yang buat akan mengecek cookie whatsauth, jika tidak terdapat cookie PASETO maka akan di redirect ke halaman login whatsauth (30)
* Setiap controller di backend sudah dilengkapi dengan pengecekan header PASETO (30)
* Pull Request ke repo ini dengan Judul 7-KELAS-NPM-NAMA, menyertakan file README.md dalam folder Chapter07/KELAS/NPM/ yang berisi :
  * Daftar URL github pages frontend 
  * URL laman depan github pages dengan whatsauth
  * Skrinsutan dari setiap url frontend yang sudah berhasil 
