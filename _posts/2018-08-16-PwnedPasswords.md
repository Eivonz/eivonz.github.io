---
layout: default
title: PwnedPasswords
date: 2018-08-15
categories: 
    - "javascript"
    - "pwnedpasswords"
---


### PwnedPasswords
Trying out Troy Hunts PwnedPasswords service, for previously known password breaches, using only hash prefix. [PwnedPasswords](https://haveibeenpwned.com/API/v2#PwnedPasswords)




<br/>
<input type="password" class="mypassword" placeholder="Enter password"/>
<pre class="passwordresult"></pre>
<button class="checkPassword">Check my password</button>




<script
	crossorigin="anonymous"
	integrity=""
	type="application/javascript"
	src="/assets/files/sha1pwnedpass.js"
></script>

<script>

	var api = "https://api.pwnedpasswords.com/range/";

	window.onload = function() {


		document.querySelector('.checkPassword').onclick = function() {

			var pwd = document.querySelector('.mypassword').value;
			
			if (!pwd || pwd == '') return;
			
			var hash = Sha1.hash(pwd).toUpperCase();
//			console.log(pwd, hash);
			
			var prefix = hash.substring(0, 5);
//			console.log("Prefix: ", prefix);
			
//			var regEx = new RegExp("^"+hash+":.*$", "gmi");
//			console.log(regEx);

			var url = api + prefix;
			fetch(url).then(function(response) {
				return response.text();
			})
			.then(function(data) {
//				console.log(data);
				
				for (var f, e = hash.substring(5, 40), u = data.split("\n"), t = 0, r = 0; r < u.length; r++)
					f = u[r].split(":")[0],
					f === e && (t = parseInt(u[r].split(":")[1]));
	
				if (t > 0) {
					document.querySelector('.passwordresult').innerHTML = "Found password in atleast %s breaches!".replace("%s", t);
				} else {
					document.querySelector('.passwordresult').innerHTML = "Your password was NOT found in previous breaches!";
				}
			
			})
			.catch(function(e) {
				console.log(e);
			});

		}


	};


</script>
