# morphball
Demo of the parallax effect. Use right / left for speed, up / down for "camera tilt", space for jump

<head>
    <script src="http://www.html5canvastutorials.com/libraries/kinetic-v3.6.0.js">
    </script>
    <script>

        window.onload = function(){	 
         
        //	var canvas = new Kinetic.Stage("container", 300, 200));
        //	var ctx = new Kinetic.Layer();
        	
            var canvas = document.getElementById("container", 800, 600);
            var ctx = canvas.getContext("2d");
         
            var widthImg = 800;
            var heightImg = 600;
            
            var mount1_x;
            var mount2_x;
            var trees_x;
            var ground_x;
            
            var bg;
            var moon;
            var mount1;
            var mount2;
            var trees;
            var ground;
            var ball = [];
            
            var frameNum = 1;
            
            var wait = 0;
            var ballX = -100;
            var ballY = 0;

	    var mx = 1;
	    var my = 0;
            var jump = "OFF";
	    var jumpY = 0;
	    var jumpY2 = -50;

	window.onkeydown = keydownControl;


function keydownControl(e) {
    if(e.keyCode==37) {
        mx <= 1 ? mx : mx--;
    } else
    if (e.keyCode==38) {
        my <= -15 ? my : my--;
    } else if
    (e.keyCode==39) {
        mx >= 20 ? mx : mx++;
    } else
    if (e.keyCode==40) {
        my >= 0 ? my : my++;
    } else
    if (e.keyCode==32) {
        jump = "ON";
    }
}

            init();

             
            function init(){
      
             
            	// init background 
            	bg = new Image();
                bg.src="stars.png";
                bg.onload = function(){
            	       //ctx.drawImage(bg,0,0);
                      // Now you can pass the `img` object to various functions
                };     
                       
               	moon = new Image();
                moon.src="moon.png";
                moon.onload = function(){
            	       //ctx.drawImage(moon,200,20);
                      // Now you can pass the `img` object to various functions
                };     
             
            	// init cloud
            	mount1 = new Image();
            	mount1.src = "back_1.png";
            	mount1.onload = function(){
           		   mount1_x = mount1.width;
            	   //ctx.drawImage(mount1, mount1_x, 120);
            	};
            	
            	mount2 = new Image();
            	mount2.src = "back_2.png";
            	mount2.onload = function(){
           		   mount2_x = mount2.width;
            	   //ctx.drawImage(mount2, mount2_x, 60);
            	};
            	            	
            	trees = new Image();
            	trees.src = "trees.png";
            	trees.onload = function(){
           		   trees_x = trees.width;
            	   //ctx.drawImage(trees, trees_x, 20);
            	};
            	            	
            	ground = new Image();
            	ground.src = "ground.png";
            	ground.onload = function(){
           		   ground_x = ground.width;
            	   //ctx.drawImage(ground, ground_x, 0);
            	};
            
               var i;
               for (i = 1; i <=	24; i++) {
            	   ball[i] = new Image();
		   if (i >= 10) {
		   	ball[i].src = "rolling_ball_" + i + ".png";
		   }
		   else {
         	   	ball[i].src = "rolling_ball_0" + i + ".png";
		   }
			
		   //ball[i].width = 50;
		   //ball[i].height = 50;

             	   ball[i].onload = function(){
                   	//ctx.drawImage(ball[i], 520, 735);
               	   }
			
               }
            	
             
            	return setInterval(main_loop, 10);
            }
             
            function update(){
            	mount1_x -= 0.1 * mx;
            	if (mount1_x < 0 ) {
            		mount1_x = mount1.width;
            	}
            	mount2_x -= 0.3 * mx;
            	if (mount2_x < 0 ) {
            		mount2_x = mount2.width;
            	}
            	trees_x -= 0.9 * mx;
            	if (trees_x < 0 ) {
            		trees_x = trees.width;
            	}
            	ground_x -= 2.7 * mx;
            	if (ground_x < 0 ) {
            		ground_x = ground.width;
            	}
            }
             
            function draw() {
		var posNeg;
		var stretch = 1.01 - mx * 0.01;
            		if (jump == "ON"){
				stretch = 1;						
				if (jumpY2 <= 50) {
					posNeg = jumpY2 == 0 ? 0 : jumpY2 / Math.abs(jumpY2);
					ballY = ballY + jumpY2 * jumpY2 * posNeg / 100;
					jumpY2++;
				}
				else {
					jumpY2 = -50;
					jump = "OFF";
				}
			}
			jumpY = jump == "OFF" ? 0 : jumpY + jumpY2 * jumpY2 * posNeg / 3000;

            	ctx.drawImage(bg, 0, 0);
            	ctx.drawImage(moon, 200, 20);
            	ctx.drawImage(mount1, mount1_x, 120 + my + jumpY);
            	ctx.drawImage(mount1, mount1_x-mount1.width, 120 + my + jumpY);
            	ctx.drawImage(mount1, mount1_x+mount1.width, 120 + my + jumpY);
            	ctx.drawImage(mount2, mount2_x, 60 - my - jumpY);
            	ctx.drawImage(mount2, mount2_x-mount2.width, 60 - my - jumpY);
            	ctx.drawImage(mount2, mount2_x+mount2.width, 60 - my - jumpY);
            	ctx.drawImage(trees, trees_x, 20 - my * 3 - jumpY * 3);
            	ctx.drawImage(trees, trees_x-trees.width, 20 - my * 3 - jumpY * 3);
            	ctx.drawImage(trees, trees_x+trees.width, 20 - my * 3 - jumpY * 3);
            	ctx.drawImage(ground, ground_x, 0 - my * 6 - jumpY * 6);
            	ctx.drawImage(ground, ground_x-ground.width, 0 - my * 6 - jumpY * 6);
            	ctx.drawImage(ground, ground_x+ground.width, 0 - my * 6 - jumpY * 6);
            	 
           	 if (wait >= 6 - mx) {
           	   if (frameNum < 24) {
            	       frameNum++;
         	   }
         	   else {
         	       frameNum = 1;
      	         }
      	       wait = 0;
 	       }

 	       wait++;
 	       if (ballX > widthImg + 100) {
 	          ballX = -100;
 	       }
 	       else {
 	          ballX++;
         }

         	   ctx.drawImage(ball[frameNum], ballX, 451 - my * 6 + ballY + (50 - 50 * stretch), 50 / stretch, 50 * stretch);
            	
            }
             
            function main_loop() {  
            	draw();
            	update();
        //        ctx.draw();  
        //        canvas.add(ctx);
            }
        };
    </script>

<style type="text/css">

</style>

</head>
<body bgcolor="black">// onmousedown="return false;">
	<table border="0" align="center">
	        <tr>
			<td colspan="2">
				<p align="center">
					<font size="5" face="arial" color="aliceblue">
						Morphball
					</font>
				</p>
			</td>
		</tr>	        
		<tr>
			<td colspan="2">
					<canvas id="container" width="800" height="600">
   		            </canvas>
			</td>
		</tr>	        
		<tr>
			<td colspan="2">
				<p align="right">
					<font size="2" face="arial" color="aliceblue" >
						//GM & KG � 2012
					</font>
				</p>
			</td>
		</tr>
			
	</table>
		
</body>

