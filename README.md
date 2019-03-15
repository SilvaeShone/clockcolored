# clockcolored<doctype html>
	<html>
		<head>
			<title></title>

			<style>

			html, body {
				height:100%;
				margin:0;
				padding:0;
			}

		
		body {
			background-color:#030d59;
		}

		#clock {
			position:absolute;
			top:50%;
			left:50%;
			-transform: translate(-50%, -50%);
			-webkit-transform: translate(-50%, -50%);
			-moz-transform: translate(-50%, -50%);
			-o-transform: translate(-50%, -50%);
			-ms-transform: translate(-50%, -50%);
		color:#FFFFFF;
		/*display:none;*/
	}

		.visibleText{
			font-family:'AvenirNextCondensed-UltraLight', sans-serif;
			font-size:15vw;
			text-align:center;
			opacity:0.9;
		}

		#selectors {
			text-align: center;
		}

		#selectors a {
			display:inline-block;
			background-color: #CCCCCC;
			color:#FFFFFF;
			width:49%;
			height: 20px;
			opacity:0.5;

		}

		#selectors a.active{
			opacity:.8;
		}
		#selectors a:hover{
			opacity:1;
		}

		.noSelect {
			user-select: none;
			-webkit-user-select: none;
			-khtml-user-select: none;
			-moz-user-select:none;
			-ms-user-select: none;
			-o-user-select: none;

			-webkit-tap-highlight-color: 0;
			-webkit-touch-callout: none;
		}
	</style>
<!--<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>-->
<script src="js/jquery.min.js" type="text/javascript" charset="utf-8"></script>
<script src="js/jquery.color.js" type="text/javascript" charset="utf-8"></script>

<script>

	$(document). ready(  function(){

		function clockUpdate(){
			//create date object
			var date = new Date();
			//parse hours, minutes, and seconds
			var hours = date. getHours();
			var minutes = date. getMinutes();
			var seconds = date. getSeconds();

			var timeString =padZero(hours) + ":" + padZero(minutes) + ":" + padZero(seconds);

			console.log(timeString);


			$("#time").html(timeString);

			var color = "#" + timeToHex(hours, minutes, seconds);

			$("#hex").html( color );

			//$('body').css({'background-color': color});

			$('body').animate({'background-color': color},  800,  function(){

				blackOrWhiteTextColorBasedOnBackground();

			});

			

		};

		clockUpdate();
		setInterval(clockUpdate, 1000);
		
		//convert time to hex string
                function timeToHex(hours, minutes, seconds) {
                    /* determine percentage of current/total time: current hour of 24, current minute of 60, etc.
                     * multiply that percentage * 255: hours = red, minutes = green, seconds = blue
                     * convert to base16 string to get hex couplet
                     * pad single digits with leading zero
                     * return all as string of 6 characters
                     */
                    return padZero(parseInt((hours / 24) * 255, 10).toString(16)) +
                           padZero(parseInt((minutes / 60) * 255, 10).toString(16)) +
                           padZero(parseInt((seconds / 60) * 255, 10).toString(16));

                           
                }

		function padZero(val){
			var value = val.toString();
			return (value.length < 2)? "0" + val : value;

		}

		$('#selectors a').click(function() {
			$( $(this).attr('href')).show().siblings().hide();
			$ (this).addClass('active').siblings().removeClass('active');//light up the bottom when click on it

		});

			//var output;

			//if (value.length < 2){
			//	output = "0" + value;
			//}	else {
			//	output = value;
			//}
			//return output
			                //change color of text based on background color
                function blackOrWhiteTextColorBasedOnBackground() {
                    //get color in rgb format, as well as locations of first and last char of the string
                    //e.g. "rgb(255, 255, 255)", 4, and 17
                    var color = $('body').css('background-color');
                    var startIndex = color.indexOf('(') + 1;
                    var lastIndex = color.indexOf(')');
                    
                    //pull string of numbers only from color (using start and end parens as indeces)
                    //  and create array of values by splitting around comma delimiter
                    var values = color.slice(startIndex,lastIndex).split(',');
                    
                    //using the brightness index, determine color contrast value
                    var o = Math.round( ( (parseInt(values[0]) * 299) + 
                                          (parseInt(values[1]) * 587) + 
                                          (parseInt(values[2]) * 114) ) / 1000);
                    //if brighter than the middle composite color value (255/2), make text black. else make white
                    (o > 127) ? $('#hex, #time').css('color', 'black') : $('#hex, #time').css('color', 'white');
                    //(o > 127) ? $('#time').css('color', 'black') : $('#time').css('color', 'white');
                }
		}
		

	  );
	//setInterval(showString, 1000);
</script>
<script>

	function showString(){
		console.log(new Date())

	}
		showString();

		setInterval(showString, 1000);


	

</script>



			<!-- start clock -->
			<div id="clock">
				<div id="textDisplay">
					<div id="time" class="visibleText noSelect">
						00:00:00
					</div>
					<div id="hex" class="visibleText noSelect" style="display: none;">
						#ffffff
					</div>
				</div>


				<div id="selectors">
					<a href="#time" class="active"></a> <a href="#hex"></a>
				</div>

				
			</div>
		</body>
	</html>
