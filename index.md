<!doctype html>

<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Whale Beach Surf</title>
        <meta name="description" content="Whale Beach Surf Report">
        <meta name="author" content="Armin Aschoff">
        
        <!--
            <link rel="stylesheet" href="https://www.w3schools.com/w3css/4/w3.css">
        -->

        <script>
            var globals = {activeFrame:undefined, clientWidth:0, youtubeAspectRation:1.7778};

            function updateColumnWidth() {
                //updates column width to match current display area, and refreshes the active iFrame

                let root = document.documentElement;
                let maxColumnWidth = window.getComputedStyle(root).getPropertyValue('--max-column-width');
                let clientWidth = root.clientWidth;
                globals.clientWidth = clientWidth;
                let setWidth = Math.min(maxColumnWidth, clientWidth);
                console.log("Default:", maxColumnWidth,"  Client Width:", clientWidth, "  So setting to:", setWidth);
                root.style.setProperty('--main-column-width',  setWidth + "px");

                //fix up background image
                document.getElementById("backgroundImage").width = clientWidth;

                //fix up youtube
                let youtubeFrame = document.getElementById("youtubeFrame");
                youtubeFrame.width = setWidth;
                youtubeFrame.height = setWidth / globals.youtubeAspectRation;

                //resize all the frame containers
                let tabcontent = document.getElementsByClassName("tabcontent");
                for (let i = 0; i < tabcontent.length; i++) {
                    tabcontent[i].width = setWidth+"px";
                }

                //resize all the weather iFrames
                let iFrameContent = document.getElementsByClassName("weatherFrame");
                for (let i = 0; i < iFrameContent.length; i++) {
                    iFrameContent[i].width = setWidth+"px";
                }

                //and refresh the active iFrame
                if (globals.activeFrame) {
                    //alert("Refreshing: " + globals.activeFrame);
                    let iframe = document.getElementById(globals.activeFrame);
                    iframe.src = iframe.src;
                }
            }

            window.onorientationchange = updateColumnWidth;

            window.onresize = function() {
                //filter out resize events triggered by iPhone scrolling...
                if (document.documentElement.clientWidth !== globals.clientWidth) {
                    globals.clientWidth = document.documentElement.clientWidth;
                    //alert("Resize");
                    updateColumnWidth();
                }
            }
        </script>
        
        <style>
            :root {
                --max-column-width: 800;
                --main-column-width: 640px;
            }   


            html, body {
                height: 100%;
                margin: 0;
            }

            body {
                background-color: #000000;
            }            

            .background-div {
                position: fixed;
                overflow: hidden;
                top:0;
                z-index: -1;
            }

            .background-div img {
                width: 100%;
                height: 100%;
            }

            #header {
                box-sizing: border-box;
                background-color: #00000044;
                text-align: center;
                background-image: linear-gradient(#0000EEFF, #0000EE00);
                letter-spacing: 2px;
                word-spacing: 3px;
                font-style: italic;
                color: whitesmoke; 
                font-family: Arial, Helvetica, sans-serif;
            }

            .centered-div {
                margin: auto;                
            }

            .pagecolumn {
                width: var(--main-column-width);
                box-sizing: border-box;
            }

            #weatherTabs {
                display: block; 
                padding-top: 30px;
                opacity: 0.96;
                filter: alpha(opacity=96); /* For IE8 and earlier */  
            }
            

            
            /* Style the tab */
            .tab {
                overflow: hidden;
                background-color: #aca6a6;
            }

            /* Style the buttons inside the tab */
            .tab button {
                background-color: #f0f0f0;
                float: left;
                border: none;
                margin-left: 3px;
                border-right: 3px;
                outline: none;
                cursor: pointer;
                padding: 14px 16px;
                transition: 0.3s;
                font-size: 13px;
            }

            /* Change background color of buttons on hover */
            .tab button:hover {
                background-color: #ddd;
            }

           /* Style the tab content */
           .tabcontent {
                display: none;
                box-sizing: border-box;
                background-color: #00000000;
                width: var(--main-column-width);
                overflow: visible;
                margin: auto;
            }
            
            /* Create an active/current tablink class */
            .tab button.active {
                background-color: #ccc;
            }
            

        </style>
        <!--
            <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
        -->
        
    </head>

    <body onload="updateColumnWidth()">

        <div class="background-div"><img id="backgroundImage" src="beach_picture.jpg"></div>

        <div id="header">
                <h1>Whale Beach Report</h1>
        </div>
/*
        <iframe width="560" height="315" src="https://www.youtube.com/embed/eLOeV0P3Gww" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        
        */
        <div class="pagecolumn centered-div">
            <div id="youtubeDiv" >
                <frameset>
                    //<iframe id="youtubeFrame" src="https://www.youtube.com/embed/live_stream?channel=UCuULfrk9IhLJc8KkvL0Sucw&autoplay=true" frameborder="0" autoplay allowfullscreen allow="accelerometer; autoplay; encrypted-media; gyroscope;"></iframe>
                     <iframe width="560" height="315" src="https://www.youtube.com/embed/eLOeV0P3Gww" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                    <noframes>
                        <body>Your browser does not support frames.</body>
                    </noframes>
                </frameset>
            </div>
            
            <div id="weatherTabs" >

                <div class="tab">
                    <button class="tablinks w3-bottombar" id="defaultOpen" onclick="openTab(event, 'windy', 'id_windy_iFrame')">Windy.com</button>
                    <button class="tablinks w3-bottombar" onclick="openTab(event, 'willyWeather', 'id_willyWeather_iFrame')">Willy Weather</button>
                    <button class="tablinks w3-bottombar" onclick="openTab(event, 'weatherunderground','id_weatherunderground_iFrame')">Weather Underground</button>
                </div>

                <div id="windy" class="tabcontent">
                        <frameset>
                            <iframe class="weatherFrame" id="id_windy_iFrame" height="800px" frameborder="0"src="https://embed.windy.com/embed2.html?lat=-33.610771&lon=151.331654&zoom=11&level=surface&overlay=waves&menu=&message=&marker=&calendar=&pressure=&type=map&location=coordinates&detail=true&detailLat=-33.610771&detailLon=151.331654&metricWind=m%2Fs&metricTemp=%C2%B0C&radarRange=-1" frameborder="0"></iframe>
                            <noframes>
                                <body>Your browser does not support frames.</body>
                            </noframes>
                        </frameset>
                </div>    

                <div id="willyWeather" class="tabcontent">
                        <frameset>
                            <iframe class="weatherFrame" id="id_willyWeather_iFrame" height="2000px" frameborder="0" src="https://www.willyweather.com.au/graphs.html?graph=outlook:1,location:2249,series=order:0,id:sunrisesunset,type:forecast,series=order:1,id:swell-height,type:forecast,series=order:2,id:swell-period,type:forecast,series=order:3,id:wind,type:forecast,series=order:4,id:tides,type:forecast" ></iframe>
                            <noframes>
                                <body>Your browser does not support frames.</body>
                            </noframes>
                        </frameset>
                </div>    

                <div id="weatherunderground" class="tabcontent">
                        <frameset>
                            <iframe class="weatherFrame" id="id_weatherunderground_iFrame" height="4600px" frameborder="0" src="https://www.wunderground.com/personal-weather-station/dashboard?ID=INSWWHAL3" ></iframe>
                            <noframes>
                                <body>Your browser does not support frames.</body>
                            </noframes>
                        </frameset>
                </div>    
            </div>
        </div>
        <script>
            function openTab(evt, tabName, iframeID) {

                globals.activeFrame = iframeID;

                var i, tabcontent, tablinks;
                tabcontent = document.getElementsByClassName("tabcontent");
                for (i = 0; i < tabcontent.length; i++) {
                    tabcontent[i].style.display = "none";
                }
                tablinks = document.getElementsByClassName("tablinks");
                for (i = 0; i < tablinks.length; i++) {
                    tablinks[i].className = tablinks[i].className.replace(" active", "");
                    //tablinks[i].className = tablinks[i].className.replace(" w3-border-red", "");
                }
                document.getElementById(tabName).style.display = "inline-block";
                //evt.currentTarget.className += " active w3-border-red";
                evt.currentTarget.className += " active";
                
                updateColumnWidth();
            }
            
            // Get the element with id="defaultOpen" and click on it
            document.getElementById("defaultOpen").click();
        </script>
    </body>
</html>
