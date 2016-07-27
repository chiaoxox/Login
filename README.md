# Login
Button loginBtn;
EditText username, password;
Thread Login;

--------------------------------------------------------------------------------------------------

loginBtn.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				// TODO Auto-generated method stub
				Login = new Login();
				Login.start();
			}
});

---------------------------------------------------------------------------------------------------

class Login extends Thread {

		@Override
		public void run() {
		
			// 建立HttpClient用以跟伺服器溝通
			HttpClient client = new DefaultHttpClient();

			try {

				HttpPost Post = new HttpPost("網址");

				// 建立POST的變數
				List<NameValuePair> vars = new ArrayList<NameValuePair>();
				vars.add(new BasicNameValuePair("Username", username.getText().toString()));
				vars.add(new BasicNameValuePair("Password", password.getText().toString()));

				// 發出POST要求 ，以及編碼
				Post.setEntity(new UrlEncodedFormEntity(vars, HTTP.UTF_8));

				// 建立ResponseHandler,以接收伺服器回傳的訊息
				ResponseHandler<String> h = new BasicResponseHandler();

				// 將回傳的訊息轉為String
				String response = new String(client.execute(Post, h).getBytes(), HTTP.UTF_8);
				String success = "Login Succeeded";

				Looper.prepare();

				// 若回傳的訊息等於"Login Succeeded"，跳轉到另一個頁面

				if (response.equals(success)) {

					Intent i = new Intent(LoginActivity.this, MainActivity.class);
					startActivity(i);
					finish();

				}

				// 否則只顯示回傳訊息

				else {

					Toast.makeText(LoginActivity.this, response, Toast.LENGTH_LONG).show();
					username.setText("");
					password.setText("");

				}
				Looper.loop();
			}

			catch (Exception ex) {
				// 若伺服器無法與asp檔連接時的動作
				ex.printStackTrace();
			}
		}
	}
