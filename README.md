<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>AQI Display</title>

<style>

body{
background:black;
color:white;
font-family:Arial;
margin:0;
text-align:center;
}

.container{
width:420px;
margin:auto;
border:4px solid #00aaff;
}

.header{
font-size:32px;
color:#00ff66;
padding:10px;
font-weight:bold;
}

.datetime{
font-size:22px;
padding:8px;
border-top:2px solid #00aaff;
border-bottom:2px solid #00aaff;
}

table{
width:100%;
border-collapse:collapse;
}

td{
border:2px solid #00aaff;
padding:10px;
font-size:22px;
}

.label{
color:#ff66ff;
text-align:left;
padding-left:20px;
}

.value{
color:#ffcc00;
font-weight:bold;
}

.unit{
color:#00ffff;
}

</style>
</head>

<body>

<div class="container">

<div class="header" id="clientName">AQI MONITOR</div>

<div class="datetime" id="dt"></div>

<table>

<tr>
<td class="label">PM2.5</td>
<td class="value" id="pm25">--</td>
<td class="unit">µg/m³</td>
</tr>

<tr>
<td class="label">TEMP</td>
<td class="value" id="temp">--</td>
<td class="unit">DegC</td>
</tr>

<tr>
<td class="label">HUM</td>
<td class="value" id="hum">--</td>
<td class="unit">%</td>
</tr>

<tr>
<td class="label">PM10</td>
<td class="value" id="pm10">--</td>
<td class="unit">µg/m³</td>
</tr>

<tr>
<td class="label">AQI</td>
<td class="value" id="aqi">--</td>
<td class="unit">---</td>
</tr>

</table>

</div>

<script>

// GET URL PARAMETERS
const params = new URLSearchParams(window.location.search);

const client = params.get("client");
const device = params.get("device") || "11";

// CHANGE CLIENT NAME
if(client){
document.getElementById("clientName").innerText = client;
}

function updateTime(){

const now = new Date()

document.getElementById("dt").innerHTML =
now.toLocaleDateString() + " " + now.toLocaleTimeString()

}

async function fetchData(){

try{

const response = await fetch("https://aqi.rudraenterpriseshansi.workers.dev/?device=" + device)

const data = await response.json()

const p = data.parameter

document.getElementById("pm25").innerText = p.pm25.value
document.getElementById("temp").innerText = p.temperature.value
document.getElementById("hum").innerText = p.humidity.value
document.getElementById("pm10").innerText = p.pm10.value
document.getElementById("aqi").innerText = p.aqi.value

}

catch(e){

console.log("API Error", e)

}

}

setInterval(updateTime,1000)
setInterval(fetchData,20000)

updateTime()
fetchData()

</script>

</body>
</html>
