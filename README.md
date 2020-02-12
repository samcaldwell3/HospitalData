# HospitalData
### This is a visualization of publicly accessible data of operations (DRG) at different hospitals nationwide. This uses d3 to highlight trends and outliers.
<html>
<head>
    <meta charset="UTF-8">
    <title>2011 Hospital Data</title>
    <script src="https://d3js.org/d3.v5.min.js"></script>
    <script src="graph-scroll.js"></script>
    <script src="2h_plot.js" ></script>

    <style>

    body{
        max-width: 900px;
        margin: 0px auto;
        font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
    }

    h1{
        margin-left: -150px;
    }

    h1{
        text-align: left;
    }

    h4{
      margin-left:-150px;
    }

    h1{
        text-align: left;
    }


    #container{
        position: relative;
    }

    #sections{
        margin-left: -150px;
        margin-top: 500px;
        width: 250px;
        padding-bottom:300px;
    }

    #sections > section{
        opacity: .2;
        margin-bottom: 200px;
    }

    #sections > section.graph-scroll-active{
        opacity: 1;
    }

    #graph{
        margin-left: 150px;
        width: 600px;
        position: sticky;
        top: 20px;
        float: left;

    }

    #selections.hidden{
        display:none;

    }


    #scatterplot2.hidden{
        display:none;
    }



    #barchart.hidden{
        display:none;
    }

    #first_bar.hidden{
        display:none;
    }

    #newdots.hidden{
        display:none;
    }

    #footer{
        height:1000px;
    }

   #tooltip{
        position:absolute;
        width:150px;
        height:auto;
        padding:10px;
        background-color: white;
        border-radius:10px;
        box-shadow: 4px 4px 10px rgba(0,0,0,0.4);
        pointer-events: none;
    }

    #tooltip.hidden{
        display:none;
    }

    #tooltip p{
        margin:0;
        font-family: sans-serif;
        font-size:12px;
    }
    #tooltip2{
         position:absolute;
         width:150px;
         height:auto;
         padding:10px;
         background-color: white;
         border-radius:10px;
         box-shadow: 4px 4px 10px rgba(0,0,0,0.4);
         pointer-events: none;
     }

     #tooltip2.hidden{
         display:none;
     }

     #tooltip2 p{
         margin:0;
         font-family: sans-serif;
         font-size:12px;
     }
     #tooltip3{
          position:absolute;
          width:150px;
          height:auto;
          padding:10px;
          background-color: white;
          border-radius:10px;
          box-shadow: 4px 4px 10px rgba(0,0,0,0.4);
          pointer-events: none;
      }

      #tooltip3.hidden{
          display:none;
      }

      #tooltip3 p{
          margin:0;
          font-family: sans-serif;
          font-size:12px;
      }

    #graph svg{
        border:solid 1px lightgrey;
    }
    #key{
      margin-left: 800px;
      margin-right: -700px;
      width: 600px;
      position: sticky;
      top: 20px;
      float: right;
    }
    </style>
</head>
<body>
    <h1>2011 Hospital Data</h1>
    <h4>Sam Caldwell</h4>
    <div id="container">
            <div id="graph">
            </div>

    <div id="sections">

        <section>
            <h3>Total Hospitals per State</h3>
            <p>With experience comes expertise. Procedures are performed at hospitals across the country all the time, but have you ever stopped to consider what state, or even what hospital, performs the most of a given procedure in a year? Are there differences in the number of hospitals per state?</p>
            <p>This data shows the total number of hospitals in each state. The color represents cost, with white being the least expensive and dark blue being the most. </p>
            <p>It is immediately obvious that a hospital visit in Maryland is significantly less expensive than those in other states. Let's keep that in the back of our minds while we look for any other unusual trends.</p>
            <p>Let's first see how the distribution of hospitals compares to state populations.</p>
        </section>
        <section>
            <h3>Compared to State Population</h3>
            <p>The population data corresponds surprisingly well with the number of hospitals per state.</p>
            <p>No outliers here.</p>
        </section>
        <section>
            <h3>Most Common Procedure</h3>
            <p>Now Let's see what happens when we compare what the most common procedure is for each state.</p>
            <p>This shows that the total number of the most common procedures performed for each state also correspond pretty well with the population data. Perhaps we should examine if there are any hospitals that perform unusual amounts of a procedure.</p>
        </section>

        <section>
            <h3>Most Common Procedure</h3>
            <p>Here we looked at the most commonly performed procedure at each hospital within a state, and then selected the hospital that performed more of their top procedure than the other hospitals, respectively.</p>
            <p>One hospital in New York performs by far the most major joint replacements (DRG 470). Is this part of a state-wide trend? Is New York as a whole more prone to joint replacements, or is there one hospital responsible?</p>
        </section>
        <section>
            <h3>Hospitals within New York</h3>
            <div id = "selections", class = "hidden">
                State: <select id="state"></select>
            </div>
            <p>We can see that within New York, most hospitals perform less than 1,000 procedures per DRG Definition. The exception to this is the Hospital for Special Surgery which performed 3,383 joint replacements.</p>
            <p>Feel free to explore trends within other states as well.</p>
        </section>
        <section>
            <h3>Most Affordable Hospital</h3>
            <p>These charts are now colored by total hospitals per state. There does not seem to be a relationship between cost and number of hospitals.</p>
            <p>Now let's return to Maryland, which has the lowest average hospital procedure cost ($13k) compared to the other states. The next cheapest state is West Virginia, which is about 1.5 times as expensive at almost $20k. What could account for this anomaly?</p>
            <p>Maryland is the only state which uses an all-payer rate setting model. Under this model, all third parties pay the same price for services at a given hospital. This system has been in place since the 1970s, and has resulted in lower patient costs across the board.</p>
            You can find out more <a href="https://innovation.cms.gov/initiatives/Maryland-All-Payer-Model/">here</a>.
        </section>
    </div>

</div>

<div id="footer"></div>
<div id="tooltip", class = "hidden">
    <p>
    Hospitals: <span id ="Hospitals"></span><br />
    Charges: $<span id="Charges"></span><br />
    DRG: <span id="Drg"></span><br />
    </p>
</div>
<div id="tooltip2", class = "hidden">
    <p>
    Average Cost: $<span id="aveCost"></span><br />
    Hospitals: <span id="hospitals"></span><br />
    State: <span id="state"></span><br />
    </p>
</div>
<div id="tooltip3", class = "hidden">
    <p>
    State: <span id="state2"></span><br />
    DRG: <span id="DRG"></span><br />
    Total Discharges: <span id="Total_Discharges2"></span><br />
    Provider ID: <span id="provider_id"></span><br />
    Provider Name: <span id="hospital_name"></span><br />
    Average Covered Charges: $<span id="ave"></span><br />
    </p>
</div>
<script>






d3.csv("https://data.cms.gov/api/views/97k6-zzx3/rows.csv?accessType=DOWNLOAD").then(function(data){
    let drgs = d3.nest()
      .key(d=>d["Provider State"])
      .key(d=>d["DRG Definition"])
      .entries(data);


    let aveStateData = d3.nest()
      .key(function(d) { return d["Provider State"]; })
      .rollup(function(v) { return d3.mean(v, function(d) { return d[" Average Covered Charges "]; })} )
      .entries(data);

    let aveStateData2 = d3.nest()
      .key(function(d) { return d["Provider State"]; })
      .key(function(d) { return d["Provider Name"]; })
      .rollup(function(v) { return v.length })
      .entries(data);

    aveStateData =  aveStateData.map(d => ({
      state: d.key,
      aveCost: Math.round(d.value),
      hospitals: 0,
    }));
    aveStateData2 = aveStateData2.map(d => ({
      state: d.key,
      hospitals: d.values.length,
    }));

    let aveStateData3 = aveStateData.slice()
    for(let i = 0; i < aveStateData2.length; i++){
      aveStateData3[i].hospitals = (aveStateData2[i].hospitals)
    }
    const color_scale = d3.scaleSequential(d3.interpolateBlues)
      .domain([0,d3.median(data, (d)=>+d[" Average Covered Charges "]), d3.max(data, (d)=>+d[" Average Covered Charges "])]);

    const color_scale2 = d3.scaleOrdinal(d3.schemeSet3);

    const color_scale3 = d3.scaleSequential(d3.interpolateBlues)
      .domain([d3.min(aveStateData3, (d)=>+d.aveCost), d3.median(aveStateData3, (d)=>+d.aveCost), d3.max(aveStateData3, (d)=>+d.aveCost)]);

    const svg = d3.select("#graph").append("svg")
      .attr("width", 800)
      .attr("height", 700);
    let state_abbrs = ["AK","AL","AR","AZ","CA","CO","CT","DC","DE","FL","GA","HI","IA","ID","IL","IN","KS","KY","LA","MA","MD","ME","MI","MN","MO","MS","MT","NC","ND","NE","NH","NJ","NM","NV","NY","OH","OK","OR","PA","RI","SC","SD","TN","TX","UT","VA","VT","WA","WI","WV","WY"]
    const dropdown = d3.select("#state")

    for (let i = 0; i < state_abbrs.length; i ++){
      dropdown.append("option").attr("value", state_abbrs[i]).text(state_abbrs[i])
    }

    let hpdata = [];

    let refresh_data = function(){
      let e = document.getElementById("state");
      let v = e.options[e.selectedIndex].text;
      let state_name = v;

      hpdata = data.filter(function(d) {return d["Provider State"] === state_name})
      let num = state_abbrs.indexOf(state_name)

    }
    refresh_data()


    const first_bar = newchart(svg, 750, 600, data, "first_bar");
    let h_plot3 = create_h_plot2(svg, 750, 600, hpdata);
    let pop_plot = newdots(svg, 750, 600, data, "newdots");

    const gs = d3.graphScroll()
      .container(d3.select("#container"))
      .graph(d3.select("#graph"))
      .eventId('sec1_id')
      .sections(d3.selectAll("#container #sections > section"))
      .on("active", function(i){
          console.log(i)
          vis_steps[i]();
      });

    d3.selectAll("#state")
      .on("change", function(){
        const metric = this.value
        refresh_data()
        h_plot3.data(hpdata)

      })

    const f1 = function() {
      console.log("f1")
      first_bar.y_metric("Hospitals")
      first_bar.section("one")
      first_bar.color_metric("Charges")
      first_bar()
      d3.select("#newdots").classed("hidden", true)
      d3.select("#first_bar").classed("hidden", false)
      d3.select("#selections").classed("hidden", true)
      d3.select("#scatterplot2").classed("hidden", true)
      d3.select("#barchart").classed("hidden", true)

    }

    const f2 = function() {
      console.log("f2")
      pop_plot()
      first_bar.y_metric("Hospitals")
      first_bar.section("one")
      first_bar.color_metric("Charges")
      first_bar()
      d3.select("#newdots").classed("hidden", false)
      d3.select("#first_bar").classed("hidden", false)

    }

    const f3 = function() {
      console.log("f3")
      first_bar.y_metric("mostCommonCount")
      first_bar.section("two")
      first_bar.color_metric("Drg")
      first_bar()
      d3.select("#newdots").classed("hidden", true)
      d3.select("#selections").classed("hidden", true)
      d3.select("#first_bar").classed("hidden", false)
      d3.select("#scatterplot2").classed("hidden", true)
      d3.select("#barchart").classed("hidden", true)


    }
    const f4 = function() {
      console.log("f4")
      first_bar.color_metric("Drg")
      first_bar.y_metric("MaxDischarges")
      first_bar.section("two")
      first_bar()
      d3.select("#newdots").classed("hidden", true)
      d3.select("#selections").classed("hidden", true)
      d3.select("#first_bar").classed("hidden", false)
      d3.select("#barchart").classed("hidden", true)
      d3.select("#scatterplot2").classed("hidden", true)
    }

    const f5 = function() {
      console.log("f5")
      document.getElementById("state").selectedIndex = "34";
      refresh_data()
      h_plot3.data(hpdata)
      d3.select("#newdots").classed("hidden", true)
      d3.select("#selections").classed("hidden", false)
      d3.select("#first_bar").classed("hidden", true)
      d3.select("#scatterplot3").classed("hidden", true)
      d3.select("#scatterplot2").classed("hidden", false)
      d3.select("#barchart").classed("hidden", true)
    }

    const f6 = function() {
      console.log("f6")
      first_bar.y_metric("Charges")
      first_bar.section("one")
      first_bar.color_metric("Hospitals")
      first_bar()
      d3.select("#newdots").classed("hidden", true)
      d3.select("#selections").classed("hidden", true)
      d3.select("#scatterplot2").classed("hidden", true)
      d3.select("#first_bar").classed("hidden", false)
      d3.select("#scatterplot3").classed("hidden", true)
      d3.select("#barchart").classed("hidden", true)
    }

    vis_steps = [f1, f2, f3, f4, f5, f6];





});

</script>

</body>
</html>
