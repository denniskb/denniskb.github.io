---
layout: empty
permalink: /test/
---
<head>
	<meta charset="utf-8">
	<link rel="shortcut icon" href="#">
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
	<script src="../assets/js/js.cookie-2.2.1.min.js"></script>
	<style>
		#template { display: none; }
		#cert { width: 100%; max-width: 1000px; height:auto; }
		@font-face {
			font-family: "Tahoma1";
			src: url("../assets/tahoma.ttf") format("truetype");
		}
		th { font-family: Tahoma1; }
	</style>
</head>
<body>
	<img id="template" src="../images/template2.png">

	<table align="center"><tr><td align="center">
	<form>
	<table cellspacing="10">
		<tr><th align="left">Vorname</th><td><input id="vorname" type="text"></td></tr>
		<tr><th align="left">Nachname</th><td><input id="nachname" type="text"></td></tr>
		<tr><th align="left">Perso-Nr.</th><td><input id="perso" type="text"></td></tr>
		<tr><th align="left">Geb.-Datum (t/m/jjjj)</th><td><input id="birth" type="text"></td></tr>
		<tr><th align="left">Test-Datum (t/m/jjjj)</th><td><input id="date" type="text"></td></tr>
		<tr><th align="left">Test-Uhrzeit (hh:mm)</th><td><input id="time" type="text"></td></tr>
		<tr><td colspan="2" align="center" valign="middle" height="60"><a href="#" id="download">Download als PNG</a></td></tr>
	</table>
	</form>
	<canvas width="1646" height="2290" id="cert"></canvas>
	</td></tr></table>

	<script>
		function hash(s) {
			/* Simple hash function. */
			var x = 100003;
			if (s) {
				a = 0;
				/*jshint plusplus:false bitwise:false*/
				for (h = 0; h < s.length; h++) {
					c = s.charCodeAt(h);
					x = (x * 7 + c * 10007) % 10000019;
				}
			}
			return String(x).substring(0, 6);
		}

		function parseDate(dateString) {
			if (dateString.length < 1) return -1;
			var tmj = dateString.split('/');
			if (tmj.length < 3) return -1;

			var t = parseInt(tmj[0], 10);
			var m = parseInt(tmj[1], 10);
			var j = parseInt(tmj[2], 10);

			return new Date(j, m-1, t);
		}

		var id = -1;
		function context() { 
			var ctx = $("#cert")[0].getContext("2d");
			ctx.clearRect(0, 0, 1646, 2290);

			ctx.drawImage($("#template")[0], 0, 0, 1646, 2290);

			ctx.font = "30px Tahoma1";
			ctx.fillText($("#nachname").val().toUpperCase() + " " + $("#vorname").val().toUpperCase(), 75, 352);
			
			ctx.font = "27px Tahoma1";
			ctx.fillText($("#perso").val().toUpperCase(), 413, 397);
			ctx.fillText(hash($("#vorname").val() + $("#nachname").val()), 295, 437);
			ctx.fillText($("#birth").val(), 453, 475);

			id = function(){
				return Math.round(((parseDate($("#date").val()) - new Date(2021, 10, 10)) /
				       (24 * 3600 * 1000) + Math.random()) * 50) + 627951;
			}();
			ctx.fillText(id, 1353, 350);
			ctx.fillText($("#date").val(), 1353, 389);
			ctx.fillText($("#time").val(), 1360, 429);
			
			var age = function(){
				var birth = parseDate($("#birth").val());
				if (birth === -1) return -1;

				return new Date(new Date() - birth).getFullYear() - 1970;
			}();
			ctx.fillText(age, 1360, 476);
		}

		$(document).ready(function(){
			if (typeof Cookies.get("vorname", {path: '/test/'}) !== "undefined"){
				$("#vorname").val(Cookies.get("vorname", {path: '/test/'}));
				$("#nachname").val(Cookies.get("nachname", {path: '/test/'}));
				$("#perso").val(Cookies.get("perso", {path: '/test/'}));
				$("#birth").val(Cookies.get("birth", {path: '/test/'}));
				$("#date").val(Cookies.get("date", {path: '/test/'}));
				$("#time").val(Cookies.get("time", {path: '/test/'}));
			}
			setTimeout(update, 500);
		});

		function update() {
			context();
			var link = $("#download")[0];
			link.download = id + ".png";
			link.href = $("#cert")[0].toDataURL("image/png");
		}

		$("#vorname").on("keyup", function(){
			Cookies.set("vorname", $(this).val(), {expires: 60, path: '/test/'});
			update();
		});
		$("#nachname").on("keyup", function(){
			Cookies.set("nachname", $(this).val(), {expires: 60, path: '/test/'});
			update();
		});
		$("#perso").on("keyup", function(){
			Cookies.set("perso", $(this).val(), {expires: 60, path: '/test/'});
			update();
		});
		$("#birth").on("keyup", function(){
			Cookies.set("birth", $(this).val(), {expires: 60, path: '/test/'});
			update();
		});
		$("#date").on("keyup", function(){
			Cookies.set("date", $(this).val(), {expires: 60, path: '/test/'});
			update();
		});
		$("#time").on("keyup", function(){
			Cookies.set("time", $(this).val(), {expires: 60, path: '/test/'});
			update();
		});
	</script>
</body>