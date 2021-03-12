---
layout: empty
permalink: /skop/
---
<head>
	<meta charset="utf-8">
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
	<script src="../assets/js/js.cookie-2.2.1.min.js"></script>
	<style>
		#template { display: none; }
		#cert { width: 100%; max-width: 1000px; height:auto; }
	</style>
</head>
<body>
	<img id="template" src="../images/template.png">

	<table align="center"><tr><td align="center">
	<form>
	<table cellspacing="20">
		<tr><th align="left">Onoma</th><td><input id="name" type="text"></td></tr>
		<tr><th align="left">On. Patera</th><td><input id="father" type="text"></td></tr>
		<tr><td colspan="2" align="center"><a href="#" id="download">Katebase to Bebaiosh</a></td></tr>
	</table>
	</form>
	<canvas width="1400" height="1979" id="cert"></canvas>
	</td></tr></table>

	<script>
		function context() { 
			var ctx = $("#cert")[0].getContext("2d");
			ctx.clearRect(0, 0, 1400, 1979);

			ctx.drawImage($("#template")[0], 0, 0, 1400, 1979);

			var date = function(){
				var d = new Date();
				var utc = d.getTime() + (d.getTimezoneOffset() * 60000);
				// Greek time zone utc+2
				var nd = new Date(utc + (3600000*2));

				return {day: nd.getDate(), month: nd.getMonth()+1};
			}();

			ctx.font = "30px sans-serif";
			ctx.fillText(date.day, 320, 540);
			ctx.fillText(date.month, 400, 540);
			ctx.fillText(date.day, 730, 730);
			ctx.fillText(date.month, 820, 730);
			
			return ctx;
		}

		$("#template").on("load", function(){
			if (typeof Cookies.get("name") !== "undefined"){
				$("#name").val(Cookies.get("name"));
				$("#father").val(Cookies.get("father"));
			}
			updateNames();
		});

		function updateNames() {
			var ctx = context();
			ctx.fillText($("#name").val(), 430, 695);
			ctx.fillText($("#father").val(), 970, 690);

			var link = $("#download")[0];
			link.download = "bebaeosh.png";
			link.href = $("#cert")[0].toDataURL("image/png");
		}

		$("#name").on("keyup", function(){
			Cookies.set("name", $(this).val());
			updateNames();
		});
		$("#father").on("keyup", function(){
			Cookies.set("father", $(this).val());
			updateNames();
		});
	</script>
</body>