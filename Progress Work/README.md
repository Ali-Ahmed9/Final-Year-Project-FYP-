## Progress work

## Garph Visulaization

[graph.html](https://github.com/user-attachments/files/31804491/graph.html)

<html>
    <head>
        <meta charset="utf-8">
        
            <script src="lib/bindings/utils.js"></script>
            <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/vis-network/9.1.2/dist/dist/vis-network.min.css" integrity="sha512-WgxfT5LWjfszlPHXRmBWHkV2eceiWTOBvrKCNbdgDYTHrT2AeLCGbF4sZlZw3UMN3WtL0tGUoIAKsu8mllg/XA==" crossorigin="anonymous" referrerpolicy="no-referrer" />
            <script src="https://cdnjs.cloudflare.com/ajax/libs/vis-network/9.1.2/dist/vis-network.min.js" integrity="sha512-LnvoEWDFrqGHlHmDD2101OrLcbsfkrzoSpvtSQtxK3RMnRV0eOkhhBN2dXHKRrUU8p2DGRTk35n4O8nWSVe1mQ==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
            
        
<center>
<h1></h1>
</center>

<!-- <link rel="stylesheet" href="../node_modules/vis/dist/vis.min.css" type="text/css" />
<script type="text/javascript" src="../node_modules/vis/dist/vis.js"> </script>-->
        <link
          href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/css/bootstrap.min.css"
          rel="stylesheet"
          integrity="sha384-eOJMYsd53ii+scO/bJGFsiCZc+5NDVN2yr8+0RDqr0Ql0h+rP48ckxlpbzKgwra6"
          crossorigin="anonymous"
        />
        <script
          src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta3/dist/js/bootstrap.bundle.min.js"
          integrity="sha384-JEW9xMcG8R+pH31jmWH6WWP0WintQrMb4s7ZOdauHnUtxwoG2vI5DkLtS3qm9Ekf"
          crossorigin="anonymous"
        ></script>


        <center>
          <h1></h1>
        </center>
        <style type="text/css">

             #mynetwork {
                 width: 100%;
                 height: 800px;
                 background-color: #ffffff;
                 border: 1px solid lightgray;
                 position: relative;
                 float: left;
             }

             
             #loadingBar {
                 position:absolute;
                 top:0px;
                 left:0px;
                 width: 100%;
                 height: 800px;
                 background-color:rgba(200,200,200,0.8);
                 -webkit-transition: all 0.5s ease;
                 -moz-transition: all 0.5s ease;
                 -ms-transition: all 0.5s ease;
                 -o-transition: all 0.5s ease;
                 transition: all 0.5s ease;
                 opacity:1;
             }

             #bar {
                 position:absolute;
                 top:0px;
                 left:0px;
                 width:20px;
                 height:20px;
                 margin:auto auto auto auto;
                 border-radius:11px;
                 border:2px solid rgba(30,30,30,0.05);
                 background: rgb(0, 173, 246); /* Old browsers */
                 box-shadow: 2px 0px 4px rgba(0,0,0,0.4);
             }

             #border {
                 position:absolute;
                 top:10px;
                 left:10px;
                 width:500px;
                 height:23px;
                 margin:auto auto auto auto;
                 box-shadow: 0px 0px 4px rgba(0,0,0,0.2);
                 border-radius:10px;
             }

             #text {
                 position:absolute;
                 top:8px;
                 left:530px;
                 width:30px;
                 height:50px;
                 margin:auto auto auto auto;
                 font-size:22px;
                 color: #000000;
             }

             div.outerBorder {
                 position:relative;
                 top:400px;
                 width:600px;
                 height:44px;
                 margin:auto auto auto auto;
                 border:8px solid rgba(0,0,0,0.1);
                 background: rgb(252,252,252); /* Old browsers */
                 background: -moz-linear-gradient(top,  rgba(252,252,252,1) 0%, rgba(237,237,237,1) 100%); /* FF3.6+ */
                 background: -webkit-gradient(linear, left top, left bottom, color-stop(0%,rgba(252,252,252,1)), color-stop(100%,rgba(237,237,237,1))); /* Chrome,Safari4+ */
                 background: -webkit-linear-gradient(top,  rgba(252,252,252,1) 0%,rgba(237,237,237,1) 100%); /* Chrome10+,Safari5.1+ */
                 background: -o-linear-gradient(top,  rgba(252,252,252,1) 0%,rgba(237,237,237,1) 100%); /* Opera 11.10+ */
                 background: -ms-linear-gradient(top,  rgba(252,252,252,1) 0%,rgba(237,237,237,1) 100%); /* IE10+ */
                 background: linear-gradient(to bottom,  rgba(252,252,252,1) 0%,rgba(237,237,237,1) 100%); /* W3C */
                 filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#fcfcfc', endColorstr='#ededed',GradientType=0 ); /* IE6-9 */
                 border-radius:72px;
                 box-shadow: 0px 0px 10px rgba(0,0,0,0.2);
             }
             

             

             
        </style>
    </head>


    <body>
        <div class="card" style="width: 100%">
            
            
            <div id="mynetwork" class="card-body"></div>
        </div>

        
            <div id="loadingBar">
              <div class="outerBorder">
                <div id="text">0%</div>
                <div id="border">
                  <div id="bar"></div>
                </div>
              </div>
            </div>
        
        

        <script type="text/javascript">

              // initialize global variables.
              var edges;
              var nodes;
              var allNodes;
              var allEdges;
              var nodeColors;
              var originalNodes;
              var network;
              var container;
              var options, data;
              var filter = {
                  item : '',
                  property : '',
                  value : []
              };

              

              

              // This method is responsible for drawing the graph, returns the drawn network
              function drawGraph() {
                  var container = document.getElementById('mynetwork');

                  

                  // parsing and collecting nodes and edges from the python
                  nodes = new vis.DataSet([{"color": "#97c2fc", "id": "g6922", "label": "g6922", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g278", "label": "g278", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1956", "label": "g1956", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1675", "label": "g1675", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1957", "label": "g1957", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g354", "label": "g354", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g113", "label": "g113", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11483", "label": "g11483", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6071", "label": "g6071", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4940", "label": "g4940", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4173", "label": "g4173", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g9721", "label": "g9721", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g611", "label": "g611", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6907", "label": "g6907", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g290", "label": "g290", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1080", "label": "g1080", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1713", "label": "g1713", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6059", "label": "g6059", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10860", "label": "g10860", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7257", "label": "g7257", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1032", "label": "g1032", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1327", "label": "g1327", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g654", "label": "g654", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11608", "label": "g11608", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6621", "label": "g6621", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8977", "label": "g8977", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_so3", "label": "test_so3", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6823", "label": "g6823", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3047", "label": "n3047", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g416", "label": "g416", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g542", "label": "g542", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11269", "label": "g11269", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6839", "label": "g6839", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g248", "label": "g248", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1389", "label": "g1389", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1371", "label": "g1371", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1015", "label": "g1015", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n1650", "label": "n1650", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6930", "label": "g6930", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g718", "label": "g718", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8985", "label": "g8985", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8433", "label": "g8433", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11482", "label": "g11482", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g351", "label": "g351", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6832", "label": "g6832", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g186", "label": "g186", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1308", "label": "g1308", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11602", "label": "g11602", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6912", "label": "g6912", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1011", "label": "g1011", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g471", "label": "g471", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1397", "label": "g1397", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4", "label": "g4", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4177", "label": "g4177", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6656", "label": "g6656", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6830", "label": "g6830", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g207", "label": "g207", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g2602", "label": "g2602", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g756", "label": "g756", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3053", "label": "n3053", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g746", "label": "g746", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1453", "label": "g1453", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g363", "label": "g363", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6546", "label": "g6546", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7843", "label": "g7843", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g153", "label": "g153", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g575", "label": "g575", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1936", "label": "g1936", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10800", "label": "g10800", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6045", "label": "g6045", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4465", "label": "g4465", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_so4", "label": "test_so4", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10797", "label": "g10797", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g560", "label": "g560", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6471", "label": "g6471", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1861", "label": "g1861", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1444", "label": "g1444", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3064", "label": "n3064", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6507", "label": "g6507", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g330", "label": "g330", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1157", "label": "g1157", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g755", "label": "g755", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n1885", "label": "n1885", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_so1", "label": "test_so1", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5404", "label": "g5404", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6983", "label": "g6983", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7930", "label": "g7930", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6525", "label": "g6525", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6821", "label": "g6821", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3054", "label": "n3054", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1531", "label": "g1531", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1458", "label": "g1458", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6528", "label": "g6528", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4238", "label": "g4238", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1741", "label": "g1741", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10795", "label": "g10795", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g557", "label": "g557", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g426", "label": "g426", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g219", "label": "g219", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11263", "label": "g11263", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11256", "label": "g11256", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g254", "label": "g254", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4178", "label": "g4178", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g861", "label": "g861", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3050", "label": "n3050", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g243", "label": "g243", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1499", "label": "g1499", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6042", "label": "g6042", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6819", "label": "g6819", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4239", "label": "g4239", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1744", "label": "g1744", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10865", "label": "g10865", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1669", "label": "g1669", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7202", "label": "g7202", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g814", "label": "g814", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6333", "label": "g6333", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1896", "label": "g1896", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g736", "label": "g736", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8282", "label": "g8282", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3051", "label": "n3051", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1411", "label": "g1411", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11331", "label": "g11331", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g486", "label": "g486", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5396", "label": "g5396", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g256", "label": "g256", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g315", "label": "g315", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8288", "label": "g8288", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1950", "label": "g1950", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7134", "label": "g7134", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3062", "label": "n3062", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1564", "label": "g1564", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1583", "label": "g1583", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g466", "label": "g466", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6506", "label": "g6506", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8050", "label": "g8050", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g174", "label": "g174", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1265", "label": "g1265", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1786", "label": "g1786", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7302", "label": "g7302", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g794", "label": "g794", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g829", "label": "g829", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3048", "label": "n3048", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5849", "label": "g5849", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8774", "label": "g8774", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1627", "label": "g1627", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1558", "label": "g1558", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6545", "label": "g6545", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1543", "label": "g1543", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3059", "label": "n3059", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1415", "label": "g1415", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g287", "label": "g287", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6901", "label": "g6901", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11337", "label": "g11337", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g516", "label": "g516", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1801", "label": "g1801", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4293", "label": "g4293", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g2986", "label": "g2986", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3058", "label": "n3058", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g309", "label": "g309", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4500", "label": "g4500", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3046", "label": "n3046", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g225", "label": "g225", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1466", "label": "g1466", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1571", "label": "g1571", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4903", "label": "g4903", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3044", "label": "n3044", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g627", "label": "g627", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1023", "label": "g1023", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n518", "label": "n518", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1508", "label": "g1508", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g962", "label": "g962", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1163", "label": "g1163", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4330", "label": "g4330", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11392", "label": "g11392", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g981", "label": "g981", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g342", "label": "g342", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1250", "label": "g1250", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11488", "label": "g11488", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6099", "label": "g6099", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6224", "label": "g6224", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1419", "label": "g1419", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8979", "label": "g8979", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8019", "label": "g8019", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4180", "label": "g4180", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3056", "label": "n3056", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11308", "label": "g11308", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g959", "label": "g959", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g158", "label": "g158", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g452", "label": "g452", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g123", "label": "g123", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11257", "label": "g11257", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n616", "label": "n616", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g3007", "label": "g3007", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10770", "label": "g10770", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1721", "label": "g1721", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8767", "label": "g8767", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1428", "label": "g1428", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g664", "label": "g664", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g965", "label": "g965", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8649", "label": "g8649", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8820", "label": "g8820", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g622", "label": "g622", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1086", "label": "g1086", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1448", "label": "g1448", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6838", "label": "g6838", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6841", "label": "g6841", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1133", "label": "g1133", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1927", "label": "g1927", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1383", "label": "g1383", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8060", "label": "g8060", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_so2", "label": "test_so2", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1474", "label": "g1474", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1561", "label": "g1561", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1546", "label": "g1546", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6542", "label": "g6542", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1759", "label": "g1759", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8978", "label": "g8978", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1296", "label": "g1296", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7293", "label": "g7293", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7292", "label": "g7292", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1240", "label": "g1240", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6501", "label": "g6501", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1601", "label": "g1601", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1092", "label": "g1092", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1574", "label": "g1574", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6088", "label": "g6088", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11484", "label": "g11484", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g357", "label": "g357", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6093", "label": "g6093", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1095", "label": "g1095", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8772", "label": "g8772", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1436", "label": "g1436", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6918", "label": "g6918", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6928", "label": "g6928", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g281", "label": "g281", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3057", "label": "n3057", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g284", "label": "g284", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1077", "label": "g1077", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1231", "label": "g1231", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5914", "label": "g5914", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4175", "label": "g4175", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g2603", "label": "g2603", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g444", "label": "g444", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11259", "label": "g11259", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g496", "label": "g496", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11334", "label": "g11334", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11333", "label": "g11333", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1333", "label": "g1333", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11610", "label": "g11610", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g2606", "label": "g2606", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n1886", "label": "n1886", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10379", "label": "g10379", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6824", "label": "g6824", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8046", "label": "g8046", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1486", "label": "g1486", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g293", "label": "g293", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7660", "label": "g7660", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1681", "label": "g1681", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11325", "label": "g11325", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11326", "label": "g11326", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1407", "label": "g1407", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1868", "label": "g1868", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6216", "label": "g6216", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1074", "label": "g1074", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7290", "label": "g7290", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1304", "label": "g1304", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8260", "label": "g8260", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g940", "label": "g940", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6284", "label": "g6284", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1292", "label": "g1292", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8943", "label": "g8943", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1882", "label": "g1882", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6478", "label": "g6478", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4176", "label": "g4176", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g806", "label": "g806", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1718", "label": "g1718", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g396", "label": "g396", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g682", "label": "g682", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8429", "label": "g8429", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11640", "label": "g11640", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1346", "label": "g1346", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g538", "label": "g538", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1470", "label": "g1470", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g822", "label": "g822", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6526", "label": "g6526", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1791", "label": "g1791", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4274", "label": "g4274", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10721", "label": "g10721", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3045", "label": "n3045", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6524", "label": "g6524", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1589", "label": "g1589", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6283", "label": "g6283", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g2612", "label": "g2612", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1400", "label": "g1400", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g554", "label": "g554", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10718", "label": "g10718", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g572", "label": "g572", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10793", "label": "g10793", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g327", "label": "g327", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6834", "label": "g6834", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g231", "label": "g231", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1019", "label": "g1019", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8435", "label": "g8435", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1864", "label": "g1864", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g369", "label": "g369", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n1637", "label": "n1637", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3065", "label": "n3065", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6126", "label": "g6126", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1753", "label": "g1753", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6906", "label": "g6906", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g269", "label": "g269", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n1865", "label": "n1865", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10782", "label": "g10782", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5536", "label": "g5536", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10859", "label": "g10859", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1660", "label": "g1660", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g386", "label": "g386", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5421", "label": "g5421", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g9", "label": "g9", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7299", "label": "g7299", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1604", "label": "g1604", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6244", "label": "g6244", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1104", "label": "g1104", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6909", "label": "g6909", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g709", "label": "g709", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8432", "label": "g8432", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6757", "label": "g6757", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g166", "label": "g166", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4891", "label": "g4891", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10707", "label": "g10707", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1730", "label": "g1730", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1101", "label": "g1101", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4076", "label": "g4076", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1707", "label": "g1707", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10866", "label": "g10866", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1684", "label": "g1684", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g262", "label": "g262", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g704", "label": "g704", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g374", "label": "g374", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10864", "label": "g10864", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3061", "label": "n3061", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g745", "label": "g745", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1534", "label": "g1534", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6234", "label": "g6234", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n542", "label": "n542", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g32", "label": "g32", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4338", "label": "g4338", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g875", "label": "g875", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g869", "label": "g869", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11290", "label": "g11290", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_si2", "label": "test_si2", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10855", "label": "g10855", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g549", "label": "g549", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11320", "label": "g11320", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1857", "label": "g1857", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11294", "label": "g11294", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4480", "label": "g4480", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g563", "label": "g563", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6934", "label": "g6934", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n3055", "label": "n3055", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1217", "label": "g1217", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6795", "label": "g6795", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6392", "label": "g6392", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11312", "label": "g11312", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1580", "label": "g1580", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5126", "label": "g5126", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g105", "label": "g105", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10898", "label": "g10898", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1424", "label": "g1424", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11310", "label": "g11310", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4890", "label": "g4890", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7626", "label": "g7626", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g639", "label": "g639", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8024", "label": "g8024", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6253", "label": "g6253", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g2609", "label": "g2609", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6822", "label": "g6822", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1227", "label": "g1227", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g501", "label": "g501", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11266", "label": "g11266", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g401", "label": "g401", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g333", "label": "g333", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1840", "label": "g1840", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1766", "label": "g1766", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7244", "label": "g7244", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11372", "label": "g11372", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g461", "label": "g461", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10664", "label": "g10664", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1633", "label": "g1633", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8777", "label": "g8777", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7586", "label": "g7586", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11391", "label": "g11391", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g976", "label": "g976", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11486", "label": "g11486", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1432", "label": "g1432", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1737", "label": "g1737", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11328", "label": "g11328", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g530", "label": "g530", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g928", "label": "g928", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g261", "label": "g261", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5392", "label": "g5392", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1736", "label": "g1736", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6840", "label": "g6840", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10858", "label": "g10858", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1672", "label": "g1672", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6924", "label": "g6924", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g312", "label": "g312", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6500", "label": "g6500", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1678", "label": "g1678", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10862", "label": "g10862", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10663", "label": "g10663", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7297", "label": "g7297", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1914", "label": "g1914", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10798", "label": "g10798", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6026", "label": "g6026", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4556", "label": "g4556", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1289", "label": "g1289", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6198", "label": "g6198", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1504", "label": "g1504", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6551", "label": "g6551", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8769", "label": "g8769", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1958", "label": "g1958", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6929", "label": "g6929", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g302", "label": "g302", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8775", "label": "g8775", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5770", "label": "g5770", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1317", "label": "g1317", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4893", "label": "g4893", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g255", "label": "g255", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6627", "label": "g6627", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g259", "label": "g259", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6538", "label": "g6538", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8384", "label": "g8384", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8250", "label": "g8250", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g932", "label": "g932", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8041", "label": "g8041", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7183", "label": "g7183", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7032", "label": "g7032", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1356", "label": "g1356", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_si4", "label": "test_si4", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4340", "label": "g4340", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1153", "label": "g1153", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6285", "label": "g6285", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10719", "label": "g10719", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6096", "label": "g6096", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1098", "label": "g1098", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8941", "label": "g8941", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1639", "label": "g1639", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8193", "label": "g8193", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7191", "label": "g7191", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6653", "label": "g6653", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8983", "label": "g8983", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g713", "label": "g713", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6826", "label": "g6826", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7133", "label": "g7133", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n1586", "label": "n1586", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g257", "label": "g257", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g318", "label": "g318", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n711", "label": "n711", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6533", "label": "g6533", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "CK", "label": "CK", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g695", "label": "g695", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g2605", "label": "g2605", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1955", "label": "g1955", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g3069", "label": "g3069", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g826", "label": "g826", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g260", "label": "g260", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g1360", "label": "g1360", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11514", "label": "g11514", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6831", "label": "g6831", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g5763", "label": "g5763", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11376", "label": "g11376", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_se", "label": "test_se", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6123", "label": "g6123", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6728", "label": "g6728", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_si3", "label": "test_si3", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6038", "label": "g6038", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11265", "label": "g11265", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8147", "label": "g8147", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8887", "label": "g8887", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8889", "label": "g8889", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8284", "label": "g8284", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8051", "label": "g8051", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10726", "label": "g10726", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6911", "label": "g6911", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g11380", "label": "g11380", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6282", "label": "g6282", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8766", "label": "g8766", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8920", "label": "g8920", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_si5", "label": "test_si5", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6180", "label": "g6180", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g6469", "label": "g6469", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8944", "label": "g8944", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "n524", "label": "n524", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8045", "label": "g8045", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g8039", "label": "g8039", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "test_si1", "label": "test_si1", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g4892", "label": "g4892", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g10722", "label": "g10722", "shape": "dot", "size": 10}, {"color": "#97c2fc", "id": "g7590", "label": "g7590", "shape": "dot", "size": 10}]);
                  edges = new vis.DataSet([{"from": "g6922", "to": "g278", "width": 1}, {"from": "g1956", "to": "g1675", "width": 1}, {"from": "g1956", "to": "g1957", "width": 1}, {"from": "g354", "to": "g113", "width": 1}, {"from": "g354", "to": "g11483", "width": 1}, {"from": "g354", "to": "g6071", "width": 1}, {"from": "g4940", "to": "g4173", "width": 1}, {"from": "g9721", "to": "g611", "width": 1}, {"from": "g6907", "to": "g290", "width": 1}, {"from": "g1080", "to": "g1713", "width": 1}, {"from": "g1080", "to": "g6059", "width": 1}, {"from": "g10860", "to": "g1675", "width": 1}, {"from": "g7257", "to": "g1032", "width": 1}, {"from": "g1327", "to": "g654", "width": 1}, {"from": "g1327", "to": "g11608", "width": 1}, {"from": "g6621", "to": "g8977", "width": 1}, {"from": "test_so3", "to": "g6823", "width": 1}, {"from": "n3047", "to": "g1032", "width": 1}, {"from": "g416", "to": "g542", "width": 1}, {"from": "g416", "to": "g11269", "width": 1}, {"from": "g6839", "to": "g248", "width": 1}, {"from": "g1389", "to": "g1371", "width": 1}, {"from": "g1015", "to": "n1650", "width": 1}, {"from": "g1015", "to": "g6930", "width": 1}, {"from": "g718", "to": "g8985", "width": 1}, {"from": "g718", "to": "g8433", "width": 1}, {"from": "g11482", "to": "g351", "width": 1}, {"from": "g6832", "to": "g186", "width": 1}, {"from": "g1308", "to": "g611", "width": 1}, {"from": "g1308", "to": "g11602", "width": 1}, {"from": "g6912", "to": "g1011", "width": 1}, {"from": "g471", "to": "g1397", "width": 1}, {"from": "g4", "to": "g4177", "width": 1}, {"from": "g4", "to": "g6656", "width": 1}, {"from": "g6830", "to": "g207", "width": 1}, {"from": "g2602", "to": "g8977", "width": 1}, {"from": "g756", "to": "n3053", "width": 1}, {"from": "g756", "to": "g746", "width": 1}, {"from": "g1453", "to": "g363", "width": 1}, {"from": "g1453", "to": "g6546", "width": 1}, {"from": "g7843", "to": "g153", "width": 1}, {"from": "g575", "to": "g1936", "width": 1}, {"from": "g575", "to": "g10800", "width": 1}, {"from": "g575", "to": "g6045", "width": 1}, {"from": "g4465", "to": "test_so4", "width": 1}, {"from": "g10797", "to": "g560", "width": 1}, {"from": "g6471", "to": "g1861", "width": 1}, {"from": "g1444", "to": "n3064", "width": 1}, {"from": "g1444", "to": "g6507", "width": 1}, {"from": "g330", "to": "g1157", "width": 1}, {"from": "g755", "to": "g756", "width": 1}, {"from": "n1885", "to": "test_so1", "width": 1}, {"from": "n1885", "to": "g5404", "width": 1}, {"from": "n1885", "to": "g6983", "width": 1}, {"from": "n1885", "to": "g7930", "width": 1}, {"from": "n1885", "to": "g6525", "width": 1}, {"from": "g6821", "to": "n3054", "width": 1}, {"from": "g1531", "to": "g1458", "width": 1}, {"from": "g1531", "to": "g6528", "width": 1}, {"from": "g4238", "to": "g1741", "width": 1}, {"from": "g10795", "to": "g557", "width": 1}, {"from": "g426", "to": "g219", "width": 1}, {"from": "g426", "to": "g11263", "width": 1}, {"from": "g426", "to": "g11256", "width": 1}, {"from": "g6045", "to": "g254", "width": 1}, {"from": "g4178", "to": "g861", "width": 1}, {"from": "n3050", "to": "g1327", "width": 1}, {"from": "g243", "to": "g1499", "width": 1}, {"from": "g243", "to": "g6042", "width": 1}, {"from": "g243", "to": "g6819", "width": 1}, {"from": "g4239", "to": "g1744", "width": 1}, {"from": "g10865", "to": "g1669", "width": 1}, {"from": "g7202", "to": "g814", "width": 1}, {"from": "g6333", "to": "g1389", "width": 1}, {"from": "g1896", "to": "g736", "width": 1}, {"from": "g1896", "to": "g8282", "width": 1}, {"from": "n3051", "to": "g1411", "width": 1}, {"from": "g11331", "to": "g486", "width": 1}, {"from": "g5396", "to": "g1713", "width": 1}, {"from": "g256", "to": "g315", "width": 1}, {"from": "g8288", "to": "g1950", "width": 1}, {"from": "g7134", "to": "n3062", "width": 1}, {"from": "g1564", "to": "g1741", "width": 1}, {"from": "g1564", "to": "g6546", "width": 1}, {"from": "g1583", "to": "g466", "width": 1}, {"from": "g1583", "to": "g6506", "width": 1}, {"from": "g8050", "to": "g174", "width": 1}, {"from": "g1265", "to": "g1786", "width": 1}, {"from": "g1265", "to": "g7302", "width": 1}, {"from": "g794", "to": "g829", "width": 1}, {"from": "g794", "to": "n3048", "width": 1}, {"from": "g794", "to": "g5849", "width": 1}, {"from": "g8774", "to": "g1627", "width": 1}, {"from": "g1744", "to": "g1558", "width": 1}, {"from": "g6545", "to": "g1543", "width": 1}, {"from": "n3059", "to": "g1415", "width": 1}, {"from": "g287", "to": "g560", "width": 1}, {"from": "g287", "to": "g6901", "width": 1}, {"from": "g11337", "to": "g516", "width": 1}, {"from": "g1801", "to": "g186", "width": 1}, {"from": "g1801", "to": "g4293", "width": 1}, {"from": "g2986", "to": "n3058", "width": 1}, {"from": "g254", "to": "g309", "width": 1}, {"from": "g254", "to": "g4178", "width": 1}, {"from": "g4500", "to": "n3046", "width": 1}, {"from": "g6823", "to": "g225", "width": 1}, {"from": "g1466", "to": "g1571", "width": 1}, {"from": "g4903", "to": "n3044", "width": 1}, {"from": "g627", "to": "g1023", "width": 1}, {"from": "n518", "to": "g1508", "width": 1}, {"from": "g153", "to": "g962", "width": 1}, {"from": "g1163", "to": "n3047", "width": 1}, {"from": "g1163", "to": "g4330", "width": 1}, {"from": "g11392", "to": "g981", "width": 1}, {"from": "g342", "to": "g1250", "width": 1}, {"from": "g342", "to": "g11488", "width": 1}, {"from": "g342", "to": "g6099", "width": 1}, {"from": "g6224", "to": "g1415", "width": 1}, {"from": "g1419", "to": "g8979", "width": 1}, {"from": "g8019", "to": "g4180", "width": 1}, {"from": "g219", "to": "n3056", "width": 1}, {"from": "g11308", "to": "g959", "width": 1}, {"from": "g158", "to": "g627", "width": 1}, {"from": "g452", "to": "g123", "width": 1}, {"from": "g452", "to": "g11257", "width": 1}, {"from": "g516", "to": "g254", "width": 1}, {"from": "n616", "to": "g3007", "width": 1}, {"from": "g10770", "to": "g1721", "width": 1}, {"from": "g1861", "to": "n3054", "width": 1}, {"from": "g8767", "to": "g1428", "width": 1}, {"from": "g664", "to": "g965", "width": 1}, {"from": "g664", "to": "g8649", "width": 1}, {"from": "g8820", "to": "g622", "width": 1}, {"from": "g6071", "to": "g1086", "width": 1}, {"from": "n3054", "to": "g1448", "width": 1}, {"from": "g6838", "to": "g1397", "width": 1}, {"from": "g6841", "to": "g243", "width": 1}, {"from": "g1448", "to": "g1133", "width": 1}, {"from": "g622", "to": "g1927", "width": 1}, {"from": "g1383", "to": "g158", "width": 1}, {"from": "g1383", "to": "g6832", "width": 1}, {"from": "g8060", "to": "g158", "width": 1}, {"from": "g959", "to": "test_so2", "width": 1}, {"from": "g1474", "to": "g1080", "width": 1}, {"from": "g1561", "to": "g1546", "width": 1}, {"from": "g1561", "to": "g6542", "width": 1}, {"from": "g1759", "to": "g351", "width": 1}, {"from": "g1759", "to": "g4293", "width": 1}, {"from": "g8978", "to": "test_so4", "width": 1}, {"from": "g1296", "to": "g2602", "width": 1}, {"from": "g1296", "to": "g7293", "width": 1}, {"from": "g1296", "to": "g7292", "width": 1}, {"from": "g1508", "to": "g1240", "width": 1}, {"from": "g6501", "to": "g1601", "width": 1}, {"from": "g1092", "to": "g1574", "width": 1}, {"from": "g1092", "to": "g6088", "width": 1}, {"from": "g1092", "to": "g6912", "width": 1}, {"from": "g11484", "to": "g357", "width": 1}, {"from": "g6093", "to": "g1095", "width": 1}, {"from": "g8772", "to": "g1436", "width": 1}, {"from": "test_so2", "to": "g6918", "width": 1}, {"from": "g6928", "to": "g281", "width": 1}, {"from": "g8977", "to": "n3062", "width": 1}, {"from": "n3057", "to": "g284", "width": 1}, {"from": "g1077", "to": "g1231", "width": 1}, {"from": "g1077", "to": "g5914", "width": 1}, {"from": "g1077", "to": "g7257", "width": 1}, {"from": "g4175", "to": "g2603", "width": 1}, {"from": "g444", "to": "g1474", "width": 1}, {"from": "g444", "to": "g11259", "width": 1}, {"from": "g496", "to": "g981", "width": 1}, {"from": "g496", "to": "g11334", "width": 1}, {"from": "g496", "to": "g11333", "width": 1}, {"from": "g1333", "to": "g153", "width": 1}, {"from": "g1333", "to": "g11610", "width": 1}, {"from": "g1397", "to": "g2606", "width": 1}, {"from": "n1886", "to": "g10379", "width": 1}, {"from": "g1371", "to": "g1956", "width": 1}, {"from": "g1371", "to": "g6824", "width": 1}, {"from": "g8046", "to": "g1486", "width": 1}, {"from": "g654", "to": "g293", "width": 1}, {"from": "g654", "to": "g7660", "width": 1}, {"from": "g542", "to": "g1681", "width": 1}, {"from": "g542", "to": "g11325", "width": 1}, {"from": "g542", "to": "g11326", "width": 1}, {"from": "g1407", "to": "g1868", "width": 1}, {"from": "g1407", "to": "g6216", "width": 1}, {"from": "g6099", "to": "g1074", "width": 1}, {"from": "g2603", "to": "g486", "width": 1}, {"from": "g7290", "to": "g1304", "width": 1}, {"from": "g8260", "to": "g940", "width": 1}, {"from": "g6284", "to": "g2602", "width": 1}, {"from": "g1627", "to": "g1292", "width": 1}, {"from": "g8943", "to": "g1882", "width": 1}, {"from": "g6478", "to": "g1574", "width": 1}, {"from": "n3044", "to": "g4176", "width": 1}, {"from": "g806", "to": "g1428", "width": 1}, {"from": "g1718", "to": "g396", "width": 1}, {"from": "g1718", "to": "g5404", "width": 1}, {"from": "g682", "to": "g1296", "width": 1}, {"from": "g682", "to": "g8429", "width": 1}, {"from": "g11640", "to": "g1346", "width": 1}, {"from": "g538", "to": "g416", "width": 1}, {"from": "g538", "to": "g11326", "width": 1}, {"from": "g1470", "to": "g822", "width": 1}, {"from": "g6526", "to": "g8985", "width": 1}, {"from": "g1791", "to": "g248", "width": 1}, {"from": "g1791", "to": "g4274", "width": 1}, {"from": "g10721", "to": "n3045", "width": 1}, {"from": "g6524", "to": "g1589", "width": 1}, {"from": "g6283", "to": "g2606", "width": 1}, {"from": "g6283", "to": "g2612", "width": 1}, {"from": "g1400", "to": "g309", "width": 1}, {"from": "g8985", "to": "g554", "width": 1}, {"from": "g10718", "to": "g572", "width": 1}, {"from": "g554", "to": "g496", "width": 1}, {"from": "g554", "to": "g10793", "width": 1}, {"from": "n3062", "to": "g327", "width": 1}, {"from": "g6834", "to": "g231", "width": 1}, {"from": "g736", "to": "g1019", "width": 1}, {"from": "g736", "to": "g8435", "width": 1}, {"from": "g736", "to": "g8649", "width": 1}, {"from": "g1864", "to": "g369", "width": 1}, {"from": "n1637", "to": "n3065", "width": 1}, {"from": "g1936", "to": "g8978", "width": 1}, {"from": "g6126", "to": "g806", "width": 1}, {"from": "g4274", "to": "g1753", "width": 1}, {"from": "g1543", "to": "g315", "width": 1}, {"from": "g6906", "to": "g269", "width": 1}, {"from": "g3007", "to": "test_so1", "width": 1}, {"from": "g3007", "to": "n1865", "width": 1}, {"from": "g10782", "to": "n3065", "width": 1}, {"from": "g5536", "to": "g4175", "width": 1}, {"from": "g1304", "to": "g243", "width": 1}, {"from": "g10859", "to": "g1660", "width": 1}, {"from": "g11263", "to": "g386", "width": 1}, {"from": "g5421", "to": "g9", "width": 1}, {"from": "g1250", "to": "g1163", "width": 1}, {"from": "g1250", "to": "g7299", "width": 1}, {"from": "g6507", "to": "g1604", "width": 1}, {"from": "g6244", "to": "g1419", "width": 1}, {"from": "g4177", "to": "g1104", "width": 1}, {"from": "g1868", "to": "g4173", "width": 1}, {"from": "g1868", "to": "g6909", "width": 1}, {"from": "g709", "to": "g1092", "width": 1}, {"from": "g709", "to": "g8433", "width": 1}, {"from": "g709", "to": "g8432", "width": 1}, {"from": "g6757", "to": "g166", "width": 1}, {"from": "g1086", "to": "g1486", "width": 1}, {"from": "g4891", "to": "n3059", "width": 1}, {"from": "g10707", "to": "g1730", "width": 1}, {"from": "g814", "to": "g231", "width": 1}, {"from": "n3053", "to": "g1101", "width": 1}, {"from": "g4076", "to": "g1707", "width": 1}, {"from": "g1436", "to": "g718", "width": 1}, {"from": "g1486", "to": "g1730", "width": 1}, {"from": "g1786", "to": "g682", "width": 1}, {"from": "g10866", "to": "g1684", "width": 1}, {"from": "g6042", "to": "g262", "width": 1}, {"from": "g1095", "to": "g704", "width": 1}, {"from": "g1095", "to": "g6918", "width": 1}, {"from": "g1681", "to": "g374", "width": 1}, {"from": "g1681", "to": "g10864", "width": 1}, {"from": "n3061", "to": "g745", "width": 1}, {"from": "g315", "to": "g1534", "width": 1}, {"from": "g6234", "to": "g1411", "width": 1}, {"from": "n542", "to": "g32", "width": 1}, {"from": "g1157", "to": "n3046", "width": 1}, {"from": "g1157", "to": "g4338", "width": 1}, {"from": "g875", "to": "g869", "width": 1}, {"from": "g11290", "to": "g374", "width": 1}, {"from": "test_si2", "to": "g829", "width": 1}, {"from": "g10855", "to": "g549", "width": 1}, {"from": "g11320", "to": "g369", "width": 1}, {"from": "g1857", "to": "g9", "width": 1}, {"from": "g1857", "to": "g11294", "width": 1}, {"from": "g4480", "to": "g1133", "width": 1}, {"from": "g374", "to": "g563", "width": 1}, {"from": "g1601", "to": "g166", "width": 1}, {"from": "g704", "to": "g1265", "width": 1}, {"from": "g1675", "to": "g354", "width": 1}, {"from": "g6824", "to": "g219", "width": 1}, {"from": "g284", "to": "g426", "width": 1}, {"from": "g284", "to": "g6934", "width": 1}, {"from": "n3055", "to": "g1217", "width": 1}, {"from": "g6795", "to": "g1864", "width": 1}, {"from": "g6392", "to": "g113", "width": 1}, {"from": "g11312", "to": "g965", "width": 1}, {"from": "g369", "to": "g1580", "width": 1}, {"from": "g572", "to": "g1011", "width": 1}, {"from": "g572", "to": "g6042", "width": 1}, {"from": "g5126", "to": "g1104", "width": 1}, {"from": "g105", "to": "g1669", "width": 1}, {"from": "g105", "to": "g10898", "width": 1}, {"from": "n3065", "to": "g1424", "width": 1}, {"from": "g962", "to": "g4175", "width": 1}, {"from": "g962", "to": "g11310", "width": 1}, {"from": "g1574", "to": "g1864", "width": 1}, {"from": "g1133", "to": "g1333", "width": 1}, {"from": "g4890", "to": "n3055", "width": 1}, {"from": "g1927", "to": "g1660", "width": 1}, {"from": "g7626", "to": "g639", "width": 1}, {"from": "g8024", "to": "g822", "width": 1}, {"from": "g611", "to": "n3055", "width": 1}, {"from": "g1231", "to": "g4", "width": 1}, {"from": "g6253", "to": "g2609", "width": 1}, {"from": "g231", "to": "g557", "width": 1}, {"from": "g231", "to": "g6822", "width": 1}, {"from": "g1227", "to": "g1721", "width": 1}, {"from": "g11334", "to": "g501", "width": 1}, {"from": "g6822", "to": "test_so3", "width": 1}, {"from": "g11266", "to": "g401", "width": 1}, {"from": "g1669", "to": "test_so3", "width": 1}, {"from": "g262", "to": "g333", "width": 1}, {"from": "g262", "to": "g1840", "width": 1}, {"from": "g1766", "to": "g1801", "width": 1}, {"from": "g1766", "to": "g11602", "width": 1}, {"from": "g7244", "to": "g1023", "width": 1}, {"from": "g11372", "to": "g461", "width": 1}, {"from": "g10664", "to": "g10379", "width": 1}, {"from": "g6983", "to": "g1791", "width": 1}, {"from": "g1633", "to": "g1753", "width": 1}, {"from": "g1633", "to": "g8777", "width": 1}, {"from": "g7586", "to": "g1227", "width": 1}, {"from": "g11391", "to": "g976", "width": 1}, {"from": "g11486", "to": "g363", "width": 1}, {"from": "g1032", "to": "g1432", "width": 1}, {"from": "g1707", "to": "g1759", "width": 1}, {"from": "g1424", "to": "g1737", "width": 1}, {"from": "g1424", "to": "g6234", "width": 1}, {"from": "g1424", "to": "g6506", "width": 1}, {"from": "g11328", "to": "g530", "width": 1}, {"from": "g928", "to": "g261", "width": 1}, {"from": "g6819", "to": "n3061", "width": 1}, {"from": "g5392", "to": "g1736", "width": 1}, {"from": "g6840", "to": "g1400", "width": 1}, {"from": "g976", "to": "g709", "width": 1}, {"from": "g1217", "to": "g1589", "width": 1}, {"from": "g357", "to": "g386", "width": 1}, {"from": "g10858", "to": "g1672", "width": 1}, {"from": "g8979", "to": "g32", "width": 1}, {"from": "g1737", "to": "g1672", "width": 1}, {"from": "g1019", "to": "n3061", "width": 1}, {"from": "g1019", "to": "g6924", "width": 1}, {"from": "g6525", "to": "g1786", "width": 1}, {"from": "g1882", "to": "g312", "width": 1}, {"from": "g1580", "to": "g1736", "width": 1}, {"from": "g1580", "to": "g6500", "width": 1}, {"from": "g1678", "to": "g174", "width": 1}, {"from": "g1678", "to": "g10862", "width": 1}, {"from": "g10663", "to": "n1637", "width": 1}, {"from": "g351", "to": "g1957", "width": 1}, {"from": "n1865", "to": "g1086", "width": 1}, {"from": "g1240", "to": "g538", "width": 1}, {"from": "g1240", "to": "g7297", "width": 1}, {"from": "g1721", "to": "n3058", "width": 1}, {"from": "g563", "to": "g1914", "width": 1}, {"from": "g563", "to": "g10798", "width": 1}, {"from": "g563", "to": "g6026", "width": 1}, {"from": "g1736", "to": "n1637", "width": 1}, {"from": "g1736", "to": "g1737", "width": 1}, {"from": "g4556", "to": "g1289", "width": 1}, {"from": "g401", "to": "g1857", "width": 1}, {"from": "g7930", "to": "g1801", "width": 1}, {"from": "g6198", "to": "g1504", "width": 1}, {"from": "g6551", "to": "g1546", "width": 1}, {"from": "g32", "to": "n1865", "width": 1}, {"from": "g8769", "to": "g1407", "width": 1}, {"from": "g1958", "to": "n3047", "width": 1}, {"from": "g869", "to": "g1383", "width": 1}, {"from": "g1741", "to": "g225", "width": 1}, {"from": "g6929", "to": "g302", "width": 1}, {"from": "g8775", "to": "g1432", "width": 1}, {"from": "g186", "to": "g959", "width": 1}, {"from": "g5770", "to": "g1453", "width": 1}, {"from": "g1317", "to": "g357", "width": 1}, {"from": "g166", "to": "g501", "width": 1}, {"from": "g1292", "to": "g290", "width": 1}, {"from": "g1292", "to": "g7293", "width": 1}, {"from": "g4893", "to": "g627", "width": 1}, {"from": "g255", "to": "g312", "width": 1}, {"from": "g6627", "to": "g8979", "width": 1}, {"from": "g1023", "to": "g259", "width": 1}, {"from": "g4176", "to": "g1583", "width": 1}, {"from": "g6538", "to": "g1558", "width": 1}, {"from": "g8384", "to": "g1840", "width": 1}, {"from": "g8250", "to": "g932", "width": 1}, {"from": "g501", "to": "g262", "width": 1}, {"from": "g8041", "to": "g1499", "width": 1}, {"from": "g7183", "to": "g8978", "width": 1}, {"from": "g965", "to": "g1400", "width": 1}, {"from": "g1104", "to": "g1304", "width": 1}, {"from": "g7032", "to": "g123", "width": 1}, {"from": "g1356", "to": "g1317", "width": 1}, {"from": "g1356", "to": "g794", "width": 1}, {"from": "g333", "to": "g269", "width": 1}, {"from": "g557", "to": "g2612", "width": 1}, {"from": "n3048", "to": "g302", "width": 1}, {"from": "g1660", "to": "g278", "width": 1}, {"from": "test_si4", "to": "g1531", "width": 1}, {"from": "g4340", "to": "g1153", "width": 1}, {"from": "g639", "to": "g1684", "width": 1}, {"from": "g302", "to": "g342", "width": 1}, {"from": "g6285", "to": "g2603", "width": 1}, {"from": "g10719", "to": "n3051", "width": 1}, {"from": "g6096", "to": "g1098", "width": 1}, {"from": "g8941", "to": "g1927", "width": 1}, {"from": "g1639", "to": "g1791", "width": 1}, {"from": "g1639", "to": "g8193", "width": 1}, {"from": "g1011", "to": "n3051", "width": 1}, {"from": "g7191", "to": "g4178", "width": 1}, {"from": "g248", "to": "g1707", "width": 1}, {"from": "g248", "to": "g6045", "width": 1}, {"from": "g6653", "to": "g8983", "width": 1}, {"from": "g713", "to": "g1153", "width": 1}, {"from": "g6826", "to": "g1371", "width": 1}, {"from": "g7133", "to": "g1766", "width": 1}, {"from": "n1586", "to": "g1217", "width": 1}, {"from": "g327", "to": "g1389", "width": 1}, {"from": "g225", "to": "g281", "width": 1}, {"from": "g225", "to": "g6826", "width": 1}, {"from": "g1458", "to": "g572", "width": 1}, {"from": "g1458", "to": "g6542", "width": 1}, {"from": "g257", "to": "g318", "width": 1}, {"from": "n711", "to": "n3057", "width": 1}, {"from": "n711", "to": "g4903", "width": 1}, {"from": "g269", "to": "g401", "width": 1}, {"from": "g1713", "to": "g333", "width": 1}, {"from": "g1713", "to": "n1885", "width": 1}, {"from": "g1534", "to": "g622", "width": 1}, {"from": "g1534", "to": "g6533", "width": 1}, {"from": "g4173", "to": "g1718", "width": 1}, {"from": "g6026", "to": "g259", "width": 1}, {"from": "g1504", "to": "g1470", "width": 1}, {"from": "g8983", "to": "g318", "width": 1}, {"from": "g9", "to": "g664", "width": 1}, {"from": "g1672", "to": "g1077", "width": 1}, {"from": "g1604", "to": "g1098", "width": 1}, {"from": "g6216", "to": "g1424", "width": 1}, {"from": "g1098", "to": "g932", "width": 1}, {"from": "g1098", "to": "g6924", "width": 1}, {"from": "g278", "to": "g1436", "width": 1}, {"from": "g549", "to": "g105", "width": 1}, {"from": "CK", "to": "g1289", "width": 1}, {"from": "CK", "to": "g1882", "width": 1}, {"from": "CK", "to": "g312", "width": 1}, {"from": "CK", "to": "g452", "width": 1}, {"from": "CK", "to": "g123", "width": 1}, {"from": "CK", "to": "g207", "width": 1}, {"from": "CK", "to": "g713", "width": 1}, {"from": "CK", "to": "g1153", "width": 1}, {"from": "CK", "to": "g1744", "width": 1}, {"from": "CK", "to": "g1558", "width": 1}, {"from": "CK", "to": "g695", "width": 1}, {"from": "CK", "to": "g461", "width": 1}, {"from": "CK", "to": "g940", "width": 1}, {"from": "CK", "to": "g976", "width": 1}, {"from": "CK", "to": "g709", "width": 1}, {"from": "CK", "to": "g1092", "width": 1}, {"from": "CK", "to": "g1574", "width": 1}, {"from": "CK", "to": "g1864", "width": 1}, {"from": "CK", "to": "g369", "width": 1}, {"from": "CK", "to": "g1580", "width": 1}, {"from": "CK", "to": "g1736", "width": 1}, {"from": "CK", "to": "n1637", "width": 1}, {"from": "CK", "to": "n3065", "width": 1}, {"from": "CK", "to": "g1424", "width": 1}, {"from": "CK", "to": "g1737", "width": 1}, {"from": "CK", "to": "g1672", "width": 1}, {"from": "CK", "to": "g1077", "width": 1}, {"from": "CK", "to": "g1231", "width": 1}, {"from": "CK", "to": "g4", "width": 1}, {"from": "CK", "to": "g4177", "width": 1}, {"from": "CK", "to": "g1104", "width": 1}, {"from": "CK", "to": "g1304", "width": 1}, {"from": "CK", "to": "g243", "width": 1}, {"from": "CK", "to": "g1499", "width": 1}, {"from": "CK", "to": "g1444", "width": 1}, {"from": "CK", "to": "n3064", "width": 1}, {"from": "CK", "to": "g4180", "width": 1}, {"from": "CK", "to": "g1543", "width": 1}, {"from": "CK", "to": "g315", "width": 1}, {"from": "CK", "to": "g1534", "width": 1}, {"from": "CK", "to": "g622", "width": 1}, {"from": "CK", "to": "g1927", "width": 1}, {"from": "CK", "to": "g1660", "width": 1}, {"from": "CK", "to": "g278", "width": 1}, {"from": "CK", "to": "g1436", "width": 1}, {"from": "CK", "to": "g718", "width": 1}, {"from": "CK", "to": "g8985", "width": 1}, {"from": "CK", "to": "g554", "width": 1}, {"from": "CK", "to": "g496", "width": 1}, {"from": "CK", "to": "g981", "width": 1}, {"from": "CK", "to": "g3007", "width": 1}, {"from": "CK", "to": "test_so1", "width": 1}, {"from": "CK", "to": "g829", "width": 1}, {"from": "CK", "to": "g1095", "width": 1}, {"from": "CK", "to": "g704", "width": 1}, {"from": "CK", "to": "g1265", "width": 1}, {"from": "CK", "to": "g1786", "width": 1}, {"from": "CK", "to": "g682", "width": 1}, {"from": "CK", "to": "g1296", "width": 1}, {"from": "CK", "to": "g2602", "width": 1}, {"from": "CK", "to": "g8977", "width": 1}, {"from": "CK", "to": "n3062", "width": 1}, {"from": "CK", "to": "g327", "width": 1}, {"from": "CK", "to": "g1389", "width": 1}, {"from": "CK", "to": "g1371", "width": 1}, {"from": "CK", "to": "g1956", "width": 1}, {"from": "CK", "to": "g1675", "width": 1}, {"from": "CK", "to": "g354", "width": 1}, {"from": "CK", "to": "g113", "width": 1}, {"from": "CK", "to": "g639", "width": 1}, {"from": "CK", "to": "g1684", "width": 1}, {"from": "CK", "to": "g1639", "width": 1}, {"from": "CK", "to": "g1791", "width": 1}, {"from": "CK", "to": "g248", "width": 1}, {"from": "CK", "to": "g1707", "width": 1}, {"from": "CK", "to": "g1759", "width": 1}, {"from": "CK", "to": "g351", "width": 1}, {"from": "CK", "to": "g1957", "width": 1}, {"from": "CK", "to": "g1604", "width": 1}, {"from": "CK", "to": "g1098", "width": 1}, {"from": "CK", "to": "g932", "width": 1}, {"from": "CK", "to": "g1896", "width": 1}, {"from": "CK", "to": "g736", "width": 1}, {"from": "CK", "to": "g1019", "width": 1}, {"from": "CK", "to": "n3061", "width": 1}, {"from": "CK", "to": "g745", "width": 1}, {"from": "CK", "to": "g1419", "width": 1}, {"from": "CK", "to": "g8979", "width": 1}, {"from": "CK", "to": "g32", "width": 1}, {"from": "CK", "to": "n1865", "width": 1}, {"from": "CK", "to": "g1086", "width": 1}, {"from": "CK", "to": "g1486", "width": 1}, {"from": "CK", "to": "g1730", "width": 1}, {"from": "CK", "to": "g1504", "width": 1}, {"from": "CK", "to": "g1470", "width": 1}, {"from": "CK", "to": "g822", "width": 1}, {"from": "CK", "to": "g2609", "width": 1}, {"from": "CK", "to": "g1678", "width": 1}, {"from": "CK", "to": "g174", "width": 1}, {"from": "CK", "to": "g1766", "width": 1}, {"from": "CK", "to": "g1801", "width": 1}, {"from": "CK", "to": "g186", "width": 1}, {"from": "CK", "to": "g959", "width": 1}, {"from": "CK", "to": "test_so2", "width": 1}, {"from": "CK", "to": "g1407", "width": 1}, {"from": "CK", "to": "g1868", "width": 1}, {"from": "CK", "to": "g4173", "width": 1}, {"from": "CK", "to": "g1718", "width": 1}, {"from": "CK", "to": "g396", "width": 1}, {"from": "CK", "to": "g1015", "width": 1}, {"from": "CK", "to": "n1650", "width": 1}, {"from": "CK", "to": "n3059", "width": 1}, {"from": "CK", "to": "g1415", "width": 1}, {"from": "CK", "to": "g1227", "width": 1}, {"from": "CK", "to": "g1721", "width": 1}, {"from": "CK", "to": "n3058", "width": 1}, {"from": "CK", "to": "n3057", "width": 1}, {"from": "CK", "to": "g284", "width": 1}, {"from": "CK", "to": "g426", "width": 1}, {"from": "CK", "to": "g219", "width": 1}, {"from": "CK", "to": "n3056", "width": 1}, {"from": "CK", "to": "g806", "width": 1}, {"from": "CK", "to": "g1428", "width": 1}, {"from": "CK", "to": "g2605", "width": 1}, {"from": "CK", "to": "g1564", "width": 1}, {"from": "CK", "to": "g1741", "width": 1}, {"from": "CK", "to": "g225", "width": 1}, {"from": "CK", "to": "g281", "width": 1}, {"from": "CK", "to": "g1308", "width": 1}, {"from": "CK", "to": "g611", "width": 1}, {"from": "CK", "to": "n3055", "width": 1}, {"from": "CK", "to": "g1217", "width": 1}, {"from": "CK", "to": "g1589", "width": 1}, {"from": "CK", "to": "g1466", "width": 1}, {"from": "CK", "to": "g1571", "width": 1}, {"from": "CK", "to": "g1861", "width": 1}, {"from": "CK", "to": "n3054", "width": 1}, {"from": "CK", "to": "g1448", "width": 1}, {"from": "CK", "to": "g1133", "width": 1}, {"from": "CK", "to": "g1333", "width": 1}, {"from": "CK", "to": "g153", "width": 1}, {"from": "CK", "to": "g962", "width": 1}, {"from": "CK", "to": "g4175", "width": 1}, {"from": "CK", "to": "g2603", "width": 1}, {"from": "CK", "to": "g486", "width": 1}, {"from": "CK", "to": "g471", "width": 1}, {"from": "CK", "to": "g1397", "width": 1}, {"from": "CK", "to": "g2606", "width": 1}, {"from": "CK", "to": "g1950", "width": 1}, {"from": "CK", "to": "g756", "width": 1}, {"from": "CK", "to": "n3053", "width": 1}, {"from": "CK", "to": "g549", "width": 1}, {"from": "CK", "to": "g105", "width": 1}, {"from": "CK", "to": "g1669", "width": 1}, {"from": "CK", "to": "test_so3", "width": 1}, {"from": "CK", "to": "g1531", "width": 1}, {"from": "CK", "to": "g1458", "width": 1}, {"from": "CK", "to": "g572", "width": 1}, {"from": "CK", "to": "g1011", "width": 1}, {"from": "CK", "to": "n3051", "width": 1}, {"from": "CK", "to": "g1411", "width": 1}, {"from": "CK", "to": "g1074", "width": 1}, {"from": "CK", "to": "g444", "width": 1}, {"from": "CK", "to": "g1474", "width": 1}, {"from": "CK", "to": "g1080", "width": 1}, {"from": "CK", "to": "g1713", "width": 1}, {"from": "CK", "to": "g333", "width": 1}, {"from": "CK", "to": "g269", "width": 1}, {"from": "CK", "to": "g401", "width": 1}, {"from": "CK", "to": "g1857", "width": 1}, {"from": "CK", "to": "g9", "width": 1}, {"from": "CK", "to": "g664", "width": 1}, {"from": "CK", "to": "g965", "width": 1}, {"from": "CK", "to": "g1400", "width": 1}, {"from": "CK", "to": "g309", "width": 1}, {"from": "CK", "to": "g814", "width": 1}, {"from": "CK", "to": "g231", "width": 1}, {"from": "CK", "to": "g557", "width": 1}, {"from": "CK", "to": "g2612", "width": 1}, {"from": "CK", "to": "g869", "width": 1}, {"from": "CK", "to": "g1383", "width": 1}, {"from": "CK", "to": "g158", "width": 1}, {"from": "CK", "to": "g627", "width": 1}, {"from": "CK", "to": "g1023", "width": 1}, {"from": "CK", "to": "g259", "width": 1}, {"from": "CK", "to": "n3050", "width": 1}, {"from": "CK", "to": "g1327", "width": 1}, {"from": "CK", "to": "g654", "width": 1}, {"from": "CK", "to": "g293", "width": 1}, {"from": "CK", "to": "g1346", "width": 1}, {"from": "CK", "to": "g1633", "width": 1}, {"from": "CK", "to": "g1753", "width": 1}, {"from": "CK", "to": "g1508", "width": 1}, {"from": "CK", "to": "g1240", "width": 1}, {"from": "CK", "to": "g538", "width": 1}, {"from": "CK", "to": "g416", "width": 1}, {"from": "CK", "to": "g542", "width": 1}, {"from": "CK", "to": "g1681", "width": 1}, {"from": "CK", "to": "g374", "width": 1}, {"from": "CK", "to": "g563", "width": 1}, {"from": "CK", "to": "g1914", "width": 1}, {"from": "CK", "to": "g530", "width": 1}, {"from": "CK", "to": "g575", "width": 1}, {"from": "CK", "to": "g1936", "width": 1}, {"from": "CK", "to": "g8978", "width": 1}, {"from": "CK", "to": "test_so4", "width": 1}, {"from": "CK", "to": "g1317", "width": 1}, {"from": "CK", "to": "g357", "width": 1}, {"from": "CK", "to": "g386", "width": 1}, {"from": "CK", "to": "g1601", "width": 1}, {"from": "CK", "to": "g166", "width": 1}, {"from": "CK", "to": "g501", "width": 1}, {"from": "CK", "to": "g262", "width": 1}, {"from": "CK", "to": "g1840", "width": 1}, {"from": "CK", "to": "g8983", "width": 1}, {"from": "CK", "to": "g318", "width": 1}, {"from": "CK", "to": "g1356", "width": 1}, {"from": "CK", "to": "g794", "width": 1}, {"from": "CK", "to": "n3048", "width": 1}, {"from": "CK", "to": "g302", "width": 1}, {"from": "CK", "to": "g342", "width": 1}, {"from": "CK", "to": "g1250", "width": 1}, {"from": "CK", "to": "g1163", "width": 1}, {"from": "CK", "to": "n3047", "width": 1}, {"from": "CK", "to": "g1032", "width": 1}, {"from": "CK", "to": "g1432", "width": 1}, {"from": "CK", "to": "g1453", "width": 1}, {"from": "CK", "to": "g363", "width": 1}, {"from": "CK", "to": "g330", "width": 1}, {"from": "CK", "to": "g1157", "width": 1}, {"from": "CK", "to": "n3046", "width": 1}, {"from": "CK", "to": "n3045", "width": 1}, {"from": "CK", "to": "g928", "width": 1}, {"from": "CK", "to": "g261", "width": 1}, {"from": "CK", "to": "g516", "width": 1}, {"from": "CK", "to": "g254", "width": 1}, {"from": "CK", "to": "g4178", "width": 1}, {"from": "CK", "to": "g861", "width": 1}, {"from": "CK", "to": "g1627", "width": 1}, {"from": "CK", "to": "g1292", "width": 1}, {"from": "CK", "to": "g290", "width": 1}, {"from": "CK", "to": "n3044", "width": 1}, {"from": "CK", "to": "g4176", "width": 1}, {"from": "CK", "to": "g1583", "width": 1}, {"from": "CK", "to": "g466", "width": 1}, {"from": "CK", "to": "g1561", "width": 1}, {"from": "CK", "to": "g1546", "width": 1}, {"from": "CK", "to": "g287", "width": 1}, {"from": "CK", "to": "g560", "width": 1}, {"from": "CK", "to": "g255", "width": 1}, {"from": "CK", "to": "g2986", "width": 1}, {"from": "CK", "to": "g1955", "width": 1}, {"from": "CK", "to": "g746", "width": 1}, {"from": "CK", "to": "g1958", "width": 1}, {"from": "CK", "to": "g3069", "width": 1}, {"from": "CK", "to": "g826", "width": 1}, {"from": "CK", "to": "g257", "width": 1}, {"from": "CK", "to": "g875", "width": 1}, {"from": "CK", "to": "g260", "width": 1}, {"from": "CK", "to": "g755", "width": 1}, {"from": "CK", "to": "g256", "width": 1}, {"from": "CK", "to": "g1360", "width": 1}, {"from": "CK", "to": "g1101", "width": 1}, {"from": "g11514", "to": "g1448", "width": 1}, {"from": "g259", "to": "n3050", "width": 1}, {"from": "g1499", "to": "g1444", "width": 1}, {"from": "g1499", "to": "g6528", "width": 1}, {"from": "g1499", "to": "g6198", "width": 1}, {"from": "g207", "to": "g713", "width": 1}, {"from": "g207", "to": "g6831", "width": 1}, {"from": "g5763", "to": "g1356", "width": 1}, {"from": "g318", "to": "g1356", "width": 1}, {"from": "g11376", "to": "g466", "width": 1}, {"from": "g940", "to": "g976", "width": 1}, {"from": "g123", "to": "g207", "width": 1}, {"from": "g174", "to": "g1766", "width": 1}, {"from": "g174", "to": "g6928", "width": 1}, {"from": "test_se", "to": "g1289", "width": 1}, {"from": "test_se", "to": "g1882", "width": 1}, {"from": "test_se", "to": "g312", "width": 1}, {"from": "test_se", "to": "g452", "width": 1}, {"from": "test_se", "to": "g123", "width": 1}, {"from": "test_se", "to": "g207", "width": 1}, {"from": "test_se", "to": "g713", "width": 1}, {"from": "test_se", "to": "g1153", "width": 1}, {"from": "test_se", "to": "g1744", "width": 1}, {"from": "test_se", "to": "g1558", "width": 1}, {"from": "test_se", "to": "g695", "width": 1}, {"from": "test_se", "to": "g461", "width": 1}, {"from": "test_se", "to": "g940", "width": 1}, {"from": "test_se", "to": "g976", "width": 1}, {"from": "test_se", "to": "g709", "width": 1}, {"from": "test_se", "to": "g1092", "width": 1}, {"from": "test_se", "to": "g1574", "width": 1}, {"from": "test_se", "to": "g1864", "width": 1}, {"from": "test_se", "to": "g369", "width": 1}, {"from": "test_se", "to": "g1580", "width": 1}, {"from": "test_se", "to": "g1736", "width": 1}, {"from": "test_se", "to": "n1637", "width": 1}, {"from": "test_se", "to": "n3065", "width": 1}, {"from": "test_se", "to": "g1424", "width": 1}, {"from": "test_se", "to": "g1737", "width": 1}, {"from": "test_se", "to": "g1672", "width": 1}, {"from": "test_se", "to": "g1077", "width": 1}, {"from": "test_se", "to": "g1231", "width": 1}, {"from": "test_se", "to": "g4", "width": 1}, {"from": "test_se", "to": "g4177", "width": 1}, {"from": "test_se", "to": "g1104", "width": 1}, {"from": "test_se", "to": "g1304", "width": 1}, {"from": "test_se", "to": "g243", "width": 1}, {"from": "test_se", "to": "g1499", "width": 1}, {"from": "test_se", "to": "g1444", "width": 1}, {"from": "test_se", "to": "n3064", "width": 1}, {"from": "test_se", "to": "g4180", "width": 1}, {"from": "test_se", "to": "g1543", "width": 1}, {"from": "test_se", "to": "g315", "width": 1}, {"from": "test_se", "to": "g1534", "width": 1}, {"from": "test_se", "to": "g622", "width": 1}, {"from": "test_se", "to": "g1927", "width": 1}, {"from": "test_se", "to": "g1660", "width": 1}, {"from": "test_se", "to": "g278", "width": 1}, {"from": "test_se", "to": "g1436", "width": 1}, {"from": "test_se", "to": "g718", "width": 1}, {"from": "test_se", "to": "g8985", "width": 1}, {"from": "test_se", "to": "g554", "width": 1}, {"from": "test_se", "to": "g496", "width": 1}, {"from": "test_se", "to": "g981", "width": 1}, {"from": "test_se", "to": "g3007", "width": 1}, {"from": "test_se", "to": "test_so1", "width": 1}, {"from": "test_se", "to": "g829", "width": 1}, {"from": "test_se", "to": "g1095", "width": 1}, {"from": "test_se", "to": "g704", "width": 1}, {"from": "test_se", "to": "g1265", "width": 1}, {"from": "test_se", "to": "g1786", "width": 1}, {"from": "test_se", "to": "g682", "width": 1}, {"from": "test_se", "to": "g1296", "width": 1}, {"from": "test_se", "to": "g2602", "width": 1}, {"from": "test_se", "to": "g8977", "width": 1}, {"from": "test_se", "to": "n3062", "width": 1}, {"from": "test_se", "to": "g327", "width": 1}, {"from": "test_se", "to": "g1389", "width": 1}, {"from": "test_se", "to": "g1371", "width": 1}, {"from": "test_se", "to": "g1956", "width": 1}, {"from": "test_se", "to": "g1675", "width": 1}, {"from": "test_se", "to": "g354", "width": 1}, {"from": "test_se", "to": "g113", "width": 1}, {"from": "test_se", "to": "g639", "width": 1}, {"from": "test_se", "to": "g1684", "width": 1}, {"from": "test_se", "to": "g1639", "width": 1}, {"from": "test_se", "to": "g1791", "width": 1}, {"from": "test_se", "to": "g248", "width": 1}, {"from": "test_se", "to": "g1707", "width": 1}, {"from": "test_se", "to": "g1759", "width": 1}, {"from": "test_se", "to": "g351", "width": 1}, {"from": "test_se", "to": "g1957", "width": 1}, {"from": "test_se", "to": "g1604", "width": 1}, {"from": "test_se", "to": "g1098", "width": 1}, {"from": "test_se", "to": "g932", "width": 1}, {"from": "test_se", "to": "g1896", "width": 1}, {"from": "test_se", "to": "g736", "width": 1}, {"from": "test_se", "to": "g1019", "width": 1}, {"from": "test_se", "to": "n3061", "width": 1}, {"from": "test_se", "to": "g745", "width": 1}, {"from": "test_se", "to": "g1419", "width": 1}, {"from": "test_se", "to": "g8979", "width": 1}, {"from": "test_se", "to": "g32", "width": 1}, {"from": "test_se", "to": "n1865", "width": 1}, {"from": "test_se", "to": "g1086", "width": 1}, {"from": "test_se", "to": "g1486", "width": 1}, {"from": "test_se", "to": "g1730", "width": 1}, {"from": "test_se", "to": "g1504", "width": 1}, {"from": "test_se", "to": "g1470", "width": 1}, {"from": "test_se", "to": "g822", "width": 1}, {"from": "test_se", "to": "g2609", "width": 1}, {"from": "test_se", "to": "g1678", "width": 1}, {"from": "test_se", "to": "g174", "width": 1}, {"from": "test_se", "to": "g1766", "width": 1}, {"from": "test_se", "to": "g1801", "width": 1}, {"from": "test_se", "to": "g186", "width": 1}, {"from": "test_se", "to": "g959", "width": 1}, {"from": "test_se", "to": "test_so2", "width": 1}, {"from": "test_se", "to": "g1407", "width": 1}, {"from": "test_se", "to": "g1868", "width": 1}, {"from": "test_se", "to": "g4173", "width": 1}, {"from": "test_se", "to": "g1718", "width": 1}, {"from": "test_se", "to": "g396", "width": 1}, {"from": "test_se", "to": "g1015", "width": 1}, {"from": "test_se", "to": "n1650", "width": 1}, {"from": "test_se", "to": "n3059", "width": 1}, {"from": "test_se", "to": "g1415", "width": 1}, {"from": "test_se", "to": "g1227", "width": 1}, {"from": "test_se", "to": "g1721", "width": 1}, {"from": "test_se", "to": "n3058", "width": 1}, {"from": "test_se", "to": "n3057", "width": 1}, {"from": "test_se", "to": "g284", "width": 1}, {"from": "test_se", "to": "g426", "width": 1}, {"from": "test_se", "to": "g219", "width": 1}, {"from": "test_se", "to": "n3056", "width": 1}, {"from": "test_se", "to": "g806", "width": 1}, {"from": "test_se", "to": "g1428", "width": 1}, {"from": "test_se", "to": "g2605", "width": 1}, {"from": "test_se", "to": "g1564", "width": 1}, {"from": "test_se", "to": "g1741", "width": 1}, {"from": "test_se", "to": "g225", "width": 1}, {"from": "test_se", "to": "g281", "width": 1}, {"from": "test_se", "to": "g1308", "width": 1}, {"from": "test_se", "to": "g611", "width": 1}, {"from": "test_se", "to": "n3055", "width": 1}, {"from": "test_se", "to": "g1217", "width": 1}, {"from": "test_se", "to": "g1589", "width": 1}, {"from": "test_se", "to": "g1466", "width": 1}, {"from": "test_se", "to": "g1571", "width": 1}, {"from": "test_se", "to": "g1861", "width": 1}, {"from": "test_se", "to": "n3054", "width": 1}, {"from": "test_se", "to": "g1448", "width": 1}, {"from": "test_se", "to": "g1133", "width": 1}, {"from": "test_se", "to": "g1333", "width": 1}, {"from": "test_se", "to": "g153", "width": 1}, {"from": "test_se", "to": "g962", "width": 1}, {"from": "test_se", "to": "g4175", "width": 1}, {"from": "test_se", "to": "g2603", "width": 1}, {"from": "test_se", "to": "g486", "width": 1}, {"from": "test_se", "to": "g471", "width": 1}, {"from": "test_se", "to": "g1397", "width": 1}, {"from": "test_se", "to": "g2606", "width": 1}, {"from": "test_se", "to": "g1950", "width": 1}, {"from": "test_se", "to": "g756", "width": 1}, {"from": "test_se", "to": "n3053", "width": 1}, {"from": "test_se", "to": "g549", "width": 1}, {"from": "test_se", "to": "g105", "width": 1}, {"from": "test_se", "to": "g1669", "width": 1}, {"from": "test_se", "to": "test_so3", "width": 1}, {"from": "test_se", "to": "g1531", "width": 1}, {"from": "test_se", "to": "g1458", "width": 1}, {"from": "test_se", "to": "g572", "width": 1}, {"from": "test_se", "to": "g1011", "width": 1}, {"from": "test_se", "to": "n3051", "width": 1}, {"from": "test_se", "to": "g1411", "width": 1}, {"from": "test_se", "to": "g1074", "width": 1}, {"from": "test_se", "to": "g444", "width": 1}, {"from": "test_se", "to": "g1474", "width": 1}, {"from": "test_se", "to": "g1080", "width": 1}, {"from": "test_se", "to": "g1713", "width": 1}, {"from": "test_se", "to": "g333", "width": 1}, {"from": "test_se", "to": "g269", "width": 1}, {"from": "test_se", "to": "g401", "width": 1}, {"from": "test_se", "to": "g1857", "width": 1}, {"from": "test_se", "to": "g9", "width": 1}, {"from": "test_se", "to": "g664", "width": 1}, {"from": "test_se", "to": "g965", "width": 1}, {"from": "test_se", "to": "g1400", "width": 1}, {"from": "test_se", "to": "g309", "width": 1}, {"from": "test_se", "to": "g814", "width": 1}, {"from": "test_se", "to": "g231", "width": 1}, {"from": "test_se", "to": "g557", "width": 1}, {"from": "test_se", "to": "g2612", "width": 1}, {"from": "test_se", "to": "g869", "width": 1}, {"from": "test_se", "to": "g1383", "width": 1}, {"from": "test_se", "to": "g158", "width": 1}, {"from": "test_se", "to": "g627", "width": 1}, {"from": "test_se", "to": "g1023", "width": 1}, {"from": "test_se", "to": "g259", "width": 1}, {"from": "test_se", "to": "n3050", "width": 1}, {"from": "test_se", "to": "g1327", "width": 1}, {"from": "test_se", "to": "g654", "width": 1}, {"from": "test_se", "to": "g293", "width": 1}, {"from": "test_se", "to": "g1346", "width": 1}, {"from": "test_se", "to": "g1633", "width": 1}, {"from": "test_se", "to": "g1753", "width": 1}, {"from": "test_se", "to": "g1508", "width": 1}, {"from": "test_se", "to": "g1240", "width": 1}, {"from": "test_se", "to": "g538", "width": 1}, {"from": "test_se", "to": "g416", "width": 1}, {"from": "test_se", "to": "g542", "width": 1}, {"from": "test_se", "to": "g1681", "width": 1}, {"from": "test_se", "to": "g374", "width": 1}, {"from": "test_se", "to": "g563", "width": 1}, {"from": "test_se", "to": "g1914", "width": 1}, {"from": "test_se", "to": "g530", "width": 1}, {"from": "test_se", "to": "g575", "width": 1}, {"from": "test_se", "to": "g1936", "width": 1}, {"from": "test_se", "to": "g8978", "width": 1}, {"from": "test_se", "to": "test_so4", "width": 1}, {"from": "test_se", "to": "g1317", "width": 1}, {"from": "test_se", "to": "g357", "width": 1}, {"from": "test_se", "to": "g386", "width": 1}, {"from": "test_se", "to": "g1601", "width": 1}, {"from": "test_se", "to": "g166", "width": 1}, {"from": "test_se", "to": "g501", "width": 1}, {"from": "test_se", "to": "g262", "width": 1}, {"from": "test_se", "to": "g1840", "width": 1}, {"from": "test_se", "to": "g8983", "width": 1}, {"from": "test_se", "to": "g318", "width": 1}, {"from": "test_se", "to": "g1356", "width": 1}, {"from": "test_se", "to": "g794", "width": 1}, {"from": "test_se", "to": "n3048", "width": 1}, {"from": "test_se", "to": "g302", "width": 1}, {"from": "test_se", "to": "g342", "width": 1}, {"from": "test_se", "to": "g1250", "width": 1}, {"from": "test_se", "to": "g1163", "width": 1}, {"from": "test_se", "to": "n3047", "width": 1}, {"from": "test_se", "to": "g1032", "width": 1}, {"from": "test_se", "to": "g1432", "width": 1}, {"from": "test_se", "to": "g1453", "width": 1}, {"from": "test_se", "to": "g363", "width": 1}, {"from": "test_se", "to": "g330", "width": 1}, {"from": "test_se", "to": "g1157", "width": 1}, {"from": "test_se", "to": "n3046", "width": 1}, {"from": "test_se", "to": "n3045", "width": 1}, {"from": "test_se", "to": "g928", "width": 1}, {"from": "test_se", "to": "g261", "width": 1}, {"from": "test_se", "to": "g516", "width": 1}, {"from": "test_se", "to": "g254", "width": 1}, {"from": "test_se", "to": "g4178", "width": 1}, {"from": "test_se", "to": "g861", "width": 1}, {"from": "test_se", "to": "g1627", "width": 1}, {"from": "test_se", "to": "g1292", "width": 1}, {"from": "test_se", "to": "g290", "width": 1}, {"from": "test_se", "to": "n3044", "width": 1}, {"from": "test_se", "to": "g4176", "width": 1}, {"from": "test_se", "to": "g1583", "width": 1}, {"from": "test_se", "to": "g466", "width": 1}, {"from": "test_se", "to": "g1561", "width": 1}, {"from": "test_se", "to": "g1546", "width": 1}, {"from": "test_se", "to": "g287", "width": 1}, {"from": "test_se", "to": "g560", "width": 1}, {"from": "test_se", "to": "g255", "width": 1}, {"from": "test_se", "to": "g2986", "width": 1}, {"from": "test_se", "to": "g1955", "width": 1}, {"from": "test_se", "to": "g746", "width": 1}, {"from": "test_se", "to": "g1958", "width": 1}, {"from": "test_se", "to": "g3069", "width": 1}, {"from": "test_se", "to": "g826", "width": 1}, {"from": "test_se", "to": "g257", "width": 1}, {"from": "test_se", "to": "g875", "width": 1}, {"from": "test_se", "to": "g260", "width": 1}, {"from": "test_se", "to": "g755", "width": 1}, {"from": "test_se", "to": "g256", "width": 1}, {"from": "test_se", "to": "g1360", "width": 1}, {"from": "test_se", "to": "g1101", "width": 1}, {"from": "g6123", "to": "g4176", "width": 1}, {"from": "g1346", "to": "g1633", "width": 1}, {"from": "n3045", "to": "g928", "width": 1}, {"from": "g1730", "to": "g1504", "width": 1}, {"from": "g6728", "to": "g4177", "width": 1}, {"from": "test_si3", "to": "g1407", "width": 1}, {"from": "g4180", "to": "g1543", "width": 1}, {"from": "g1415", "to": "g1227", "width": 1}, {"from": "n1650", "to": "n3059", "width": 1}, {"from": "g1753", "to": "g1508", "width": 1}, {"from": "g6038", "to": "g261", "width": 1}, {"from": "g6831", "to": "g1383", "width": 1}, {"from": "g1950", "to": "g756", "width": 1}, {"from": "g530", "to": "g575", "width": 1}, {"from": "g11265", "to": "g396", "width": 1}, {"from": "g260", "to": "g327", "width": 1}, {"from": "g8147", "to": "g928", "width": 1}, {"from": "g8887", "to": "g695", "width": 1}, {"from": "g822", "to": "g2609", "width": 1}, {"from": "g8889", "to": "g704", "width": 1}, {"from": "g1101", "to": "g549", "width": 1}, {"from": "g861", "to": "g1627", "width": 1}, {"from": "g290", "to": "n3044", "width": 1}, {"from": "g363", "to": "g330", "width": 1}, {"from": "g363", "to": "g6093", "width": 1}, {"from": "g8284", "to": "g1914", "width": 1}, {"from": "g1589", "to": "g1466", "width": 1}, {"from": "g2609", "to": "g1678", "width": 1}, {"from": "g8051", "to": "g1470", "width": 1}, {"from": "g981", "to": "g3007", "width": 1}, {"from": "g1360", "to": "n3056", "width": 1}, {"from": "g1955", "to": "g1956", "width": 1}, {"from": "g10726", "to": "n1650", "width": 1}, {"from": "g466", "to": "g1561", "width": 1}, {"from": "g461", "to": "g940", "width": 1}, {"from": "g113", "to": "g639", "width": 1}, {"from": "g6911", "to": "g293", "width": 1}, {"from": "g1840", "to": "g8983", "width": 1}, {"from": "g11380", "to": "g471", "width": 1}, {"from": "g826", "to": "g861", "width": 1}, {"from": "g6282", "to": "g2605", "width": 1}, {"from": "g309", "to": "g814", "width": 1}, {"from": "g829", "to": "g1095", "width": 1}, {"from": "g8766", "to": "g1444", "width": 1}, {"from": "n3058", "to": "n3057", "width": 1}, {"from": "g293", "to": "g1346", "width": 1}, {"from": "g261", "to": "g330", "width": 1}, {"from": "g261", "to": "g516", "width": 1}, {"from": "g1074", "to": "g444", "width": 1}, {"from": "g1074", "to": "g6930", "width": 1}, {"from": "g1957", "to": "g1604", "width": 1}, {"from": "g1684", "to": "g1639", "width": 1}, {"from": "g1914", "to": "g530", "width": 1}, {"from": "g3069", "to": "n3050", "width": 1}, {"from": "g8920", "to": "g713", "width": 1}, {"from": "g2606", "to": "g1950", "width": 1}, {"from": "g281", "to": "g1308", "width": 1}, {"from": "test_si5", "to": "g1317", "width": 1}, {"from": "g6180", "to": "g1458", "width": 1}, {"from": "g1428", "to": "g2605", "width": 1}, {"from": "g1428", "to": "g6524", "width": 1}, {"from": "g6469", "to": "g1571", "width": 1}, {"from": "g8944", "to": "g1936", "width": 1}, {"from": "n3046", "to": "n3045", "width": 1}, {"from": "n524", "to": "n3064", "width": 1}, {"from": "g312", "to": "g452", "width": 1}, {"from": "g1289", "to": "g1882", "width": 1}, {"from": "g486", "to": "g471", "width": 1}, {"from": "n3064", "to": "g4180", "width": 1}, {"from": "g386", "to": "g1601", "width": 1}, {"from": "g1411", "to": "g1074", "width": 1}, {"from": "g1411", "to": "g6500", "width": 1}, {"from": "g1411", "to": "g6244", "width": 1}, {"from": "g2612", "to": "g869", "width": 1}, {"from": "n3056", "to": "g806", "width": 1}, {"from": "g8045", "to": "g1466", "width": 1}, {"from": "g932", "to": "g1896", "width": 1}, {"from": "g8039", "to": "g1474", "width": 1}, {"from": "g1546", "to": "g287", "width": 1}, {"from": "test_si1", "to": "g1289", "width": 1}, {"from": "g1558", "to": "g695", "width": 1}, {"from": "g746", "to": "g745", "width": 1}, {"from": "g4892", "to": "n3053", "width": 1}, {"from": "g2605", "to": "g1564", "width": 1}, {"from": "g10722", "to": "n3048", "width": 1}, {"from": "g1571", "to": "g1861", "width": 1}, {"from": "g396", "to": "g1015", "width": 1}, {"from": "g396", "to": "g11266", "width": 1}, {"from": "g1153", "to": "g1744", "width": 1}, {"from": "g7590", "to": "g1231", "width": 1}, {"from": "g1432", "to": "g1453", "width": 1}, {"from": "g695", "to": "g461", "width": 1}, {"from": "g745", "to": "g1419", "width": 1}]);

                  nodeColors = {};
                  allNodes = nodes.get({ returnType: "Object" });
                  for (nodeId in allNodes) {
                    nodeColors[nodeId] = allNodes[nodeId].color;
                  }
                  allEdges = edges.get({ returnType: "Object" });
                  // adding nodes and edges to the graph
                  data = {nodes: nodes, edges: edges};

                  var options = {
    "configure": {
        "enabled": false
    },
    "edges": {
        "color": {
            "inherit": true
        },
        "smooth": {
            "enabled": true,
            "type": "dynamic"
        }
    },
    "interaction": {
        "dragNodes": true,
        "hideEdgesOnDrag": false,
        "hideNodesOnDrag": false
    },
    "physics": {
        "enabled": true,
        "stabilization": {
            "enabled": true,
            "fit": true,
            "iterations": 1000,
            "onlyDynamicEdges": false,
            "updateInterval": 50
        }
    }
};

                  


                  

                  network = new vis.Network(container, data, options);

                  

                  

                  


                  
                      network.on("stabilizationProgress", function(params) {
                          document.getElementById('loadingBar').removeAttribute("style");
                          var maxWidth = 496;
                          var minWidth = 20;
                          var widthFactor = params.iterations/params.total;
                          var width = Math.max(minWidth,maxWidth * widthFactor);
                          document.getElementById('bar').style.width = width + 'px';
                          document.getElementById('text').innerHTML = Math.round(widthFactor*100) + '%';
                      });
                      network.once("stabilizationIterationsDone", function() {
                          document.getElementById('text').innerHTML = '100%';
                          document.getElementById('bar').style.width = '496px';
                          document.getElementById('loadingBar').style.opacity = 0;
                          // really clean the dom element
                          setTimeout(function () {document.getElementById('loadingBar').style.display = 'none';}, 500);
                      });
                  

                  return network;

              }
              drawGraph();
        </script>
    </body>
</html>


============================================================
## GNN_Trojan_Detection_Visualization


[GNN_Trojan_Detection_Visualization.html](https://github.com/user-attachments/files/31804532/GNN_Trojan_Detection_Visualization.html)
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>How GNN Detects Hardware Trojans</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&family=JetBrains+Mono:wght@400;600&display=swap');

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg:        #0B0F1A;
    --panel:     #111827;
    --panel2:    #1a2235;
    --border:    #1e2d45;
    --clean:     #10B981;
    --clean-dim: #064e3b;
    --trojan:    #EF4444;
    --trojan-dim:#7f1d1d;
    --gold:      #F59E0B;
    --blue:      #3B82F6;
    --purple:    #8B5CF6;
    --text:      #E2E8F0;
    --muted:     #64748B;
    --mono:      'JetBrains Mono', monospace;
  }

  body {
    font-family: 'Inter', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── HEADER ── */
  header {
    text-align: center;
    padding: 48px 24px 32px;
    border-bottom: 1px solid var(--border);
    background: linear-gradient(180deg, #0d1526 0%, var(--bg) 100%);
  }
  .badge {
    display: inline-block;
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.15em;
    text-transform: uppercase;
    color: var(--gold);
    border: 1px solid var(--gold);
    border-radius: 999px;
    padding: 4px 16px;
    margin-bottom: 20px;
  }
  header h1 {
    font-size: clamp(28px, 5vw, 52px);
    font-weight: 900;
    letter-spacing: -0.03em;
    line-height: 1.1;
    margin-bottom: 16px;
  }
  header h1 span.hl-clean { color: var(--clean); }
  header h1 span.hl-trojan { color: var(--trojan); }
  header p {
    font-size: 16px;
    color: var(--muted);
    max-width: 640px;
    margin: 0 auto 28px;
    line-height: 1.7;
  }

  /* ── STEP PILLS ── */
  .steps-bar {
    display: flex;
    justify-content: center;
    gap: 8px;
    flex-wrap: wrap;
    margin-top: 8px;
  }
  .step-pill {
    font-size: 12px;
    font-weight: 600;
    padding: 6px 18px;
    border-radius: 999px;
    border: 1px solid var(--border);
    color: var(--muted);
    cursor: pointer;
    transition: all 0.25s;
    user-select: none;
  }
  .step-pill.active, .step-pill:hover {
    border-color: var(--blue);
    color: var(--blue);
    background: rgba(59,130,246,0.08);
  }
  .step-pill.active { background: rgba(59,130,246,0.15); }

  /* ── MAIN LAYOUT ── */
  .main {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0;
    max-width: 1400px;
    margin: 0 auto;
    padding: 32px 24px;
  }
  @media (max-width: 900px) { .main { grid-template-columns: 1fr; } }

  .divider-v {
    width: 1px;
    background: var(--border);
    position: absolute;
    left: 50%;
    top: 0; bottom: 0;
  }

  /* ── PANEL ── */
  .panel {
    padding: 28px;
    position: relative;
  }
  .panel-clean { border-right: 1px solid var(--border); }

  .panel-label {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 20px;
  }
  .panel-label .dot {
    width: 12px; height: 12px;
    border-radius: 50%;
    flex-shrink: 0;
  }
  .dot-clean { background: var(--clean); box-shadow: 0 0 8px var(--clean); }
  .dot-trojan { background: var(--trojan); box-shadow: 0 0 8px var(--trojan); }
  .panel-label h2 { font-size: 18px; font-weight: 700; }
  .panel-label .status-tag {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    padding: 3px 10px;
    border-radius: 999px;
  }
  .tag-clean { background: rgba(16,185,129,0.15); color: var(--clean); }
  .tag-trojan { background: rgba(239,68,68,0.15); color: var(--trojan); }

  /* ── CIRCUIT CANVAS ── */
  .circuit-wrap {
    background: var(--panel);
    border-radius: 16px;
    border: 1px solid var(--border);
    padding: 20px;
    margin-bottom: 20px;
    position: relative;
    overflow: hidden;
  }
  .circuit-wrap::before {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(ellipse at 50% 0%, rgba(59,130,246,0.04), transparent 70%);
    pointer-events: none;
  }

  svg.circuit { width: 100%; height: 280px; display: block; }

  /* ── GNN PROCESS ── */
  .process-section {
    margin-bottom: 20px;
  }
  .process-title {
    font-size: 11px;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--muted);
    margin-bottom: 12px;
  }

  .gnn-layers {
    display: flex;
    gap: 6px;
    margin-bottom: 14px;
  }
  .layer-block {
    flex: 1;
    background: var(--panel2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 10px 6px;
    text-align: center;
    font-size: 10px;
    color: var(--muted);
    transition: all 0.4s;
    position: relative;
    overflow: hidden;
  }
  .layer-block.lit {
    border-color: var(--blue);
    color: var(--text);
    background: rgba(59,130,246,0.08);
  }
  .layer-block.lit::after {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, rgba(59,130,246,0.08), transparent);
  }
  .layer-block .layer-icon { font-size: 18px; margin-bottom: 4px; display: block; }
  .layer-block .layer-name { font-weight: 600; font-size: 9px; line-height: 1.3; }

  /* ── MESSAGE PASSING ANIMATION ── */
  .msg-pass {
    background: var(--panel2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 14px 16px;
    margin-bottom: 14px;
    font-size: 13px;
    line-height: 1.6;
    min-height: 64px;
    transition: all 0.3s;
    position: relative;
    overflow: hidden;
  }
  .msg-pass .msg-label {
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 6px;
  }
  .msg-label-clean { color: var(--clean); }
  .msg-label-trojan { color: var(--trojan); }

  /* ── NODE FEATURE DISPLAY ── */
  .node-features {
    display: flex;
    gap: 6px;
    flex-wrap: wrap;
    margin-bottom: 14px;
  }
  .feat-chip {
    font-family: var(--mono);
    font-size: 11px;
    font-weight: 600;
    padding: 4px 10px;
    border-radius: 6px;
    border: 1px solid transparent;
    transition: all 0.3s;
    opacity: 0.5;
  }
  .feat-chip.active { opacity: 1; }
  .chip-and   { background: rgba(239,68,68,0.15);  border-color: rgba(239,68,68,0.3);  color: #FCA5A5; }
  .chip-or    { background: rgba(59,130,246,0.15); border-color: rgba(59,130,246,0.3); color: #93C5FD; }
  .chip-xor   { background: rgba(139,92,246,0.15); border-color: rgba(139,92,246,0.3); color: #C4B5FD; }
  .chip-not   { background: rgba(245,158,11,0.15); border-color: rgba(245,158,11,0.3); color: #FCD34D; }
  .chip-in    { background: rgba(16,185,129,0.15); border-color: rgba(16,185,129,0.3); color: #6EE7B7; }
  .chip-out   { background: rgba(251,191,36,0.15); border-color: rgba(251,191,36,0.3); color: #FDE68A; }
  .chip-sig   { background: rgba(100,116,139,0.2); border-color: rgba(100,116,139,0.4); color: #CBD5E1; }
  .chip-trojan{ background: rgba(239,68,68,0.25);  border-color: rgba(239,68,68,0.6);  color: #FCA5A5; animation: pulse-trojan 1s infinite; }

  @keyframes pulse-trojan {
    0%, 100% { box-shadow: 0 0 0 0 rgba(239,68,68,0.4); }
    50%       { box-shadow: 0 0 0 6px rgba(239,68,68,0); }
  }

  /* ── RESULT CARD ── */
  .result-card {
    border-radius: 12px;
    padding: 16px 20px;
    display: flex;
    align-items: center;
    gap: 16px;
    border: 2px solid transparent;
    transition: all 0.5s;
    opacity: 0;
    transform: translateY(8px);
  }
  .result-card.visible { opacity: 1; transform: translateY(0); }
  .result-clean  { background: rgba(16,185,129,0.1);  border-color: var(--clean); }
  .result-trojan { background: rgba(239,68,68,0.1);   border-color: var(--trojan); }
  .result-icon { font-size: 36px; flex-shrink: 0; }
  .result-label { font-size: 11px; font-weight: 700; letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: 4px; }
  .result-label-clean  { color: var(--clean); }
  .result-label-trojan { color: var(--trojan); }
  .result-title { font-size: 20px; font-weight: 800; }
  .result-sub   { font-size: 12px; color: var(--muted); margin-top: 2px; }

  /* ── METRICS ROW ── */
  .metrics-row {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 24px 32px;
  }
  .metric-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 18px;
    text-align: center;
  }
  .metric-val {
    font-size: 32px;
    font-weight: 900;
    letter-spacing: -0.03em;
    margin-bottom: 4px;
  }
  .metric-label { font-size: 11px; color: var(--muted); font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; }
  .metric-sub   { font-size: 10px; color: var(--muted); margin-top: 4px; }

  /* ── EXPLAINER ── */
  .explainer {
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 24px 32px;
  }
  .explain-grid {
    display: grid;
    grid-template-columns: repeat(5,1fr);
    gap: 10px;
  }
  @media (max-width: 900px) { .explain-grid { grid-template-columns: repeat(2,1fr); } }
  .explain-card {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 16px;
  }
  .explain-num {
    font-size: 11px;
    font-weight: 800;
    color: var(--blue);
    letter-spacing: 0.1em;
    margin-bottom: 8px;
  }
  .explain-title { font-size: 13px; font-weight: 700; margin-bottom: 6px; }
  .explain-body  { font-size: 11px; color: var(--muted); line-height: 1.6; }

  /* ── LEGEND ── */
  .legend {
    max-width: 1400px;
    margin: 0 auto;
    padding: 0 24px 48px;
  }
  .legend-title { font-size: 11px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.12em; color: var(--muted); margin-bottom: 14px; }
  .legend-grid  { display: flex; gap: 10px; flex-wrap: wrap; }
  .legend-item  { display: flex; align-items: center; gap: 8px; font-size: 12px; }
  .legend-dot   { width: 10px; height: 10px; border-radius: 50%; flex-shrink: 0; }

  /* ── CONTROLS ── */
  .controls {
    display: flex;
    justify-content: center;
    gap: 12px;
    padding: 0 24px 24px;
    flex-wrap: wrap;
  }
  .btn {
    font-family: 'Inter', sans-serif;
    font-size: 13px;
    font-weight: 600;
    padding: 10px 28px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
    transition: all 0.2s;
    letter-spacing: 0.02em;
  }
  .btn-primary { background: var(--blue); color: #fff; }
  .btn-primary:hover { background: #2563eb; transform: translateY(-1px); }
  .btn-ghost { background: transparent; color: var(--muted); border: 1px solid var(--border); }
  .btn-ghost:hover { border-color: var(--blue); color: var(--blue); }

  /* ── PROGRESS BAR ── */
  .progress-wrap { max-width: 1400px; margin: 0 auto; padding: 0 24px 24px; }
  .progress-bar-outer {
    background: var(--panel);
    border: 1px solid var(--border);
    border-radius: 999px;
    height: 6px;
    overflow: hidden;
  }
  .progress-bar-inner {
    height: 100%;
    background: linear-gradient(90deg, var(--blue), var(--purple));
    border-radius: 999px;
    transition: width 0.5s ease;
    width: 0%;
  }

  /* ── NODE TOOLTIP ── */
  .tooltip {
    position: fixed;
    background: var(--panel2);
    border: 1px solid var(--border);
    border-radius: 10px;
    padding: 10px 14px;
    font-size: 12px;
    pointer-events: none;
    opacity: 0;
    transition: opacity 0.2s;
    z-index: 100;
    max-width: 200px;
    line-height: 1.5;
  }
  .tooltip.show { opacity: 1; }
  .tooltip-name { font-weight: 700; margin-bottom: 4px; }
  .tooltip-type { font-size: 10px; color: var(--muted); }

  /* ── PULSE RING ── */
  @keyframes ring {
    0%   { r: 14; opacity: 0.8; }
    100% { r: 26; opacity: 0; }
  }
  .pulse-ring { animation: ring 1.2s ease-out forwards; }

  /* ── EDGE TRAVEL ── */
  @keyframes travel {
    0%   { stroke-dashoffset: 100; opacity: 1; }
    100% { stroke-dashoffset: 0;   opacity: 0.3; }
  }
  .traveling { animation: travel 0.8s ease forwards; }

  footer {
    text-align: center;
    padding: 24px;
    border-top: 1px solid var(--border);
    font-size: 12px;
    color: var(--muted);
  }
</style>
</head>
<body>

<div class="tooltip" id="tooltip">
  <div class="tooltip-name" id="tt-name"></div>
  <div class="tooltip-type" id="tt-type"></div>
</div>

<!-- HEADER -->
<header>
  <div class="badge">Interactive Visualization</div>
  <h1>How GNN Detects<br><span class="hl-clean">Clean</span> vs <span class="hl-trojan">Trojan</span> Circuits</h1>
  <p>An interactive step-by-step visualization showing how a Graph Neural Network analyzes hardware circuits and detects malicious Hardware Trojans — no prior knowledge required.</p>
  <div class="steps-bar" id="stepBar">
    <div class="step-pill active" onclick="goStep(0)">① Verilog → Graph</div>
    <div class="step-pill" onclick="goStep(1)">② Node Encoding</div>
    <div class="step-pill" onclick="goStep(2)">③ Message Passing</div>
    <div class="step-pill" onclick="goStep(3)">④ Layer 2 Learning</div>
    <div class="step-pill" onclick="goStep(4)">⑤ Pooling</div>
    <div class="step-pill" onclick="goStep(5)">⑥ Classification</div>
  </div>
</header>

<!-- PROGRESS -->
<div class="progress-wrap">
  <div class="progress-bar-outer">
    <div class="progress-bar-inner" id="progressBar"></div>
  </div>
</div>

<!-- CONTROLS -->
<div class="controls">
  <button class="btn btn-ghost" onclick="prevStep()">← Previous</button>
  <button class="btn btn-primary" id="nextBtn" onclick="nextStep()">Next Step →</button>
  <button class="btn btn-ghost" onclick="resetAll()">↺ Reset</button>
</div>

<!-- MAIN PANELS -->
<div class="main">
  <!-- LEFT: CLEAN -->
  <div class="panel panel-clean">
    <div class="panel-label">
      <div class="dot dot-clean"></div>
      <h2>Trojan-Free Circuit</h2>
      <span class="status-tag tag-clean">CLEAN</span>
    </div>

    <div class="circuit-wrap">
      <svg class="circuit" id="svgClean" viewBox="0 0 500 260"></svg>
    </div>

    <div class="process-section">
      <div class="process-title">GNN Layer Activity</div>
      <div class="gnn-layers" id="layersClean">
        <div class="layer-block" id="lc0"><span class="layer-icon">📥</span><div class="layer-name">Input</div></div>
        <div class="layer-block" id="lc1"><span class="layer-icon">🔄</span><div class="layer-name">GCN Layer 1</div></div>
        <div class="layer-block" id="lc2"><span class="layer-icon">🔄</span><div class="layer-name">GCN Layer 2</div></div>
        <div class="layer-block" id="lc3"><span class="layer-icon">🎯</span><div class="layer-name">TopK Pool</div></div>
        <div class="layer-block" id="lc4"><span class="layer-icon">📊</span><div class="layer-name">Readout</div></div>
        <div class="layer-block" id="lc5"><span class="layer-icon">✅</span><div class="layer-name">Classify</div></div>
      </div>
    </div>

    <div class="process-section">
      <div class="process-title">Node Features Detected</div>
      <div class="node-features" id="featClean">
        <span class="feat-chip chip-in"  id="fc-in">INPUT</span>
        <span class="feat-chip chip-and" id="fc-and">AND</span>
        <span class="feat-chip chip-or"  id="fc-or">OR</span>
        <span class="feat-chip chip-not" id="fc-not">NOT</span>
        <span class="feat-chip chip-xor" id="fc-xor">XOR</span>
        <span class="feat-chip chip-sig" id="fc-sig">SIGNAL</span>
        <span class="feat-chip chip-out" id="fc-out">OUTPUT</span>
      </div>
    </div>

    <div class="msg-pass" id="msgClean">
      <div class="msg-label msg-label-clean">GNN ANALYSIS</div>
      <span id="msgCleanText">Press "Next Step" to begin the analysis...</span>
    </div>

    <div class="result-card result-clean" id="resultClean">
      <div class="result-icon">✅</div>
      <div>
        <div class="result-label result-label-clean">GNN Decision</div>
        <div class="result-title">NON-TROJAN</div>
        <div class="result-sub">Normal data flow • No anomalous nodes • Clean circuit</div>
      </div>
    </div>
  </div>

  <!-- RIGHT: TROJAN -->
  <div class="panel">
    <div class="panel-label">
      <div class="dot dot-trojan"></div>
      <h2>Trojan-Infected Circuit</h2>
      <span class="status-tag tag-trojan">INFECTED</span>
    </div>

    <div class="circuit-wrap">
      <svg class="circuit" id="svgTrojan" viewBox="0 0 500 260"></svg>
    </div>

    <div class="process-section">
      <div class="process-title">GNN Layer Activity</div>
      <div class="gnn-layers" id="layersTrojan">
        <div class="layer-block" id="lt0"><span class="layer-icon">📥</span><div class="layer-name">Input</div></div>
        <div class="layer-block" id="lt1"><span class="layer-icon">🔄</span><div class="layer-name">GCN Layer 1</div></div>
        <div class="layer-block" id="lt2"><span class="layer-icon">🔄</span><div class="layer-name">GCN Layer 2</div></div>
        <div class="layer-block" id="lt3"><span class="layer-icon">🎯</span><div class="layer-name">TopK Pool</div></div>
        <div class="layer-block" id="lt4"><span class="layer-icon">📊</span><div class="layer-name">Readout</div></div>
        <div class="layer-block" id="lt5"><span class="layer-icon">⚠️</span><div class="layer-name">Classify</div></div>
      </div>
    </div>

    <div class="process-section">
      <div class="process-title">Node Features Detected</div>
      <div class="node-features" id="featTrojan">
        <span class="feat-chip chip-in"     id="ft-in">INPUT</span>
        <span class="feat-chip chip-and"    id="ft-and">AND</span>
        <span class="feat-chip chip-or"     id="ft-or">OR</span>
        <span class="feat-chip chip-not"    id="ft-not">NOT</span>
        <span class="feat-chip chip-xor"    id="ft-xor">XOR</span>
        <span class="feat-chip chip-sig"    id="ft-sig">SIGNAL</span>
        <span class="feat-chip chip-out"    id="ft-out">OUTPUT</span>
        <span class="feat-chip chip-trojan" id="ft-trj" style="display:none">⚠ TROJAN NODE</span>
      </div>
    </div>

    <div class="msg-pass" id="msgTrojan">
      <div class="msg-label msg-label-trojan">GNN ANALYSIS</div>
      <span id="msgTrojanText">Press "Next Step" to begin the analysis...</span>
    </div>

    <div class="result-card result-trojan" id="resultTrojan">
      <div class="result-icon">🚨</div>
      <div>
        <div class="result-label result-label-trojan">GNN Decision</div>
        <div class="result-title">TROJAN DETECTED!</div>
        <div class="result-sub">Anomalous nodes found • Suspicious data flow • Trojan trigger identified</div>
      </div>
    </div>
  </div>
</div>

<!-- METRICS -->
<div class="metrics-row">
  <div class="metric-card">
    <div class="metric-val" style="color:var(--clean)">100%</div>
    <div class="metric-label">Accuracy</div>
    <div class="metric-sub">On Trust-Hub dataset</div>
  </div>
  <div class="metric-card">
    <div class="metric-val" style="color:var(--blue)">100%</div>
    <div class="metric-label">TPR</div>
    <div class="metric-sub">All trojans detected</div>
  </div>
  <div class="metric-card">
    <div class="metric-val" style="color:var(--purple)">100%</div>
    <div class="metric-label">TNR</div>
    <div class="metric-sub">Zero false alarms</div>
  </div>
  <div class="metric-card">
    <div class="metric-val" style="color:var(--gold)">1.000</div>
    <div class="metric-label">F1 Score</div>
    <div class="metric-sub">Perfect F1</div>
  </div>
</div>

<!-- HOW IT WORKS EXPLAINER -->
<div class="explainer">
  <div class="legend-title" style="margin-bottom:16px">How GNN Works — Step by Step</div>
  <div class="explain-grid">
    <div class="explain-card">
      <div class="explain-num">STEP 01</div>
      <div class="explain-title">Verilog → Graph</div>
      <div class="explain-body">The hardware circuit written in Verilog code is parsed by pyverilog, which automatically extracts a Data Flow Graph (DFG) representing all gates, signals and their connections.</div>
    </div>
    <div class="explain-card">
      <div class="explain-num">STEP 02</div>
      <div class="explain-title">Node Encoding</div>
      <div class="explain-body">Each node (gate) gets a number: AND=11, OR=14, XOR=20, INPUT=1, OUTPUT=16, SIGNAL=5. This converts the graph into numbers the neural network can process.</div>
    </div>
    <div class="explain-card">
      <div class="explain-num">STEP 03</div>
      <div class="explain-title">Message Passing</div>
      <div class="explain-body">Each node sends its feature to neighbors. Every node receives messages from ALL its connected neighbors and combines them — like asking everyone around you what they see.</div>
    </div>
    <div class="explain-card">
      <div class="explain-num">STEP 04</div>
      <div class="explain-title">Deep Learning</div>
      <div class="explain-body">After 2 GCN layers, each node has learned about gates 2 hops away. The network now understands local circuit patterns and can recognize normal vs abnormal structures.</div>
    </div>
    <div class="explain-card">
      <div class="explain-num">STEP 05</div>
      <div class="explain-title">Classification</div>
      <div class="explain-body">The learned graph representation passes through a final classifier that outputs TROJAN (1) or NON-TROJAN (0). Trojan circuits have extra trigger nodes with suspicious connectivity.</div>
    </div>
  </div>
</div>

<!-- LEGEND -->
<div class="legend">
  <div class="legend-title">Node Type Legend</div>
  <div class="legend-grid">
    <div class="legend-item"><div class="legend-dot" style="background:#10B981"></div> INPUT — Circuit input port</div>
    <div class="legend-item"><div class="legend-dot" style="background:#EF4444"></div> AND — Logic AND gate</div>
    <div class="legend-item"><div class="legend-dot" style="background:#3B82F6"></div> OR — Logic OR gate</div>
    <div class="legend-item"><div class="legend-dot" style="background:#8B5CF6"></div> XOR — Logic XOR gate</div>
    <div class="legend-item"><div class="legend-dot" style="background:#F59E0B"></div> NOT — Logic NOT gate</div>
    <div class="legend-item"><div class="legend-dot" style="background:#64748B"></div> SIGNAL — Internal wire</div>
    <div class="legend-item"><div class="legend-dot" style="background:#FBBF24"></div> OUTPUT — Circuit output</div>
    <div class="legend-item"><div class="legend-dot" style="background:#DC2626;box-shadow:0 0 6px #DC2626"></div> TROJAN — Malicious node ⚠</div>
  </div>
</div>

<footer>
  Hardware Trojan Detection Using GNN &nbsp;|&nbsp; Ali Ahmed — 22ABELT0950 &nbsp;|&nbsp; UET Department of Electronics Engineering &nbsp;|&nbsp; Supervised by Mam Mehmoona Gul
</footer>

<script>
// ══════════════════════════════════════
// CIRCUIT DEFINITIONS
// ══════════════════════════════════════

// Clean circuit nodes
const cleanNodes = [
  { id:'in1',  x:60,  y:50,  type:'input',  label:'IN A',   color:'#10B981', code:1  },
  { id:'in2',  x:60,  y:120, type:'input',  label:'IN B',   color:'#10B981', code:1  },
  { id:'in3',  x:60,  y:190, type:'input',  label:'IN C',   color:'#10B981', code:1  },
  { id:'and1', x:180, y:80,  type:'and',    label:'AND',    color:'#EF4444', code:11 },
  { id:'or1',  x:180, y:170, type:'or',     label:'OR',     color:'#3B82F6', code:14 },
  { id:'not1', x:290, y:80,  type:'not',    label:'NOT',    color:'#F59E0B', code:12 },
  { id:'xor1', x:290, y:170, type:'xor',    label:'XOR',    color:'#8B5CF6', code:20 },
  { id:'and2', x:390, y:125, type:'and',    label:'AND',    color:'#EF4444', code:11 },
  { id:'out1', x:470, y:125, type:'output', label:'OUT',    color:'#FBBF24', code:16 },
];

const cleanEdges = [
  {from:'in1', to:'and1'}, {from:'in2', to:'and1'},
  {from:'in2', to:'or1'},  {from:'in3', to:'or1'},
  {from:'and1',to:'not1'}, {from:'or1', to:'xor1'},
  {from:'not1',to:'and2'}, {from:'xor1',to:'and2'},
  {from:'and2',to:'out1'},
];

// Trojan circuit nodes (same base + trojan nodes)
const trojanNodes = [
  { id:'in1',  x:60,  y:50,  type:'input',  label:'IN A',   color:'#10B981', code:1  },
  { id:'in2',  x:60,  y:120, type:'input',  label:'IN B',   color:'#10B981', code:1  },
  { id:'in3',  x:60,  y:190, type:'input',  label:'IN C',   color:'#10B981', code:1  },
  { id:'and1', x:180, y:80,  type:'and',    label:'AND',    color:'#EF4444', code:11 },
  { id:'or1',  x:180, y:170, type:'or',     label:'OR',     color:'#3B82F6', code:14 },
  { id:'not1', x:290, y:80,  type:'not',    label:'NOT',    color:'#F59E0B', code:12 },
  { id:'xor1', x:290, y:170, type:'xor',    label:'XOR',    color:'#8B5CF6', code:20 },
  { id:'and2', x:390, y:125, type:'and',    label:'AND',    color:'#EF4444', code:11 },
  { id:'out1', x:470, y:125, type:'output', label:'OUT',    color:'#FBBF24', code:16 },
  // TROJAN NODES
  { id:'trg1', x:160, y:230, type:'trojan', label:'TRIGGER',color:'#DC2626', code:99, isTrojan:true },
  { id:'trg2', x:280, y:230, type:'trojan', label:'COUNTER',color:'#DC2626', code:99, isTrojan:true },
  { id:'pay1', x:390, y:220, type:'trojan', label:'PAYLOAD',color:'#DC2626', code:99, isTrojan:true },
];

const trojanEdges = [
  {from:'in1', to:'and1'}, {from:'in2', to:'and1'},
  {from:'in2', to:'or1'},  {from:'in3', to:'or1'},
  {from:'and1',to:'not1'}, {from:'or1', to:'xor1'},
  {from:'not1',to:'and2'}, {from:'xor1',to:'and2'},
  {from:'and2',to:'out1'},
  // TROJAN EDGES
  {from:'in3', to:'trg1', isTrojan:true},
  {from:'trg1',to:'trg2', isTrojan:true},
  {from:'trg2',to:'pay1', isTrojan:true},
  {from:'pay1',to:'out1', isTrojan:true},
];

// ══════════════════════════════════════
// DRAW CIRCUITS
// ══════════════════════════════════════

function nodePos(nodes, id) {
  return nodes.find(n => n.id === id);
}

function drawCircuit(svgId, nodes, edges, highlightNodes=[], highlightEdges=false) {
  const svg = document.getElementById(svgId);
  svg.innerHTML = '';

  const ns = 'http://www.w3.org/2000/svg';

  // Draw edges first
  edges.forEach((e, ei) => {
    const from = nodePos(nodes, e.from);
    const to   = nodePos(nodes, e.to);
    if (!from || !to) return;

    const mx = (from.x + to.x) / 2;

    const path = document.createElementNS(ns, 'path');
    const d = `M${from.x},${from.y} C${mx},${from.y} ${mx},${to.y} ${to.x},${to.y}`;
    path.setAttribute('d', d);
    path.setAttribute('fill', 'none');
    path.setAttribute('stroke', e.isTrojan ? '#DC2626' : '#1e2d45');
    path.setAttribute('stroke-width', e.isTrojan ? '2.5' : '1.5');
    path.setAttribute('stroke-dasharray', e.isTrojan ? '5,3' : 'none');
    if (highlightEdges && !e.isTrojan) {
      path.setAttribute('stroke', '#3B82F6');
      path.setAttribute('stroke-width', '2');
    }
    path.setAttribute('opacity', '0.8');
    path.setAttribute('data-edge', ei);
    svg.appendChild(path);
  });

  // Draw nodes
  nodes.forEach(n => {
    const g = document.createElementNS(ns, 'g');
    g.setAttribute('transform', `translate(${n.x},${n.y})`);
    g.style.cursor = 'pointer';

    const isHl   = highlightNodes.includes(n.id);
    const isTrj  = n.isTrojan;

    // Glow ring for highlighted
    if (isHl) {
      const ring = document.createElementNS(ns, 'circle');
      ring.setAttribute('r', '22');
      ring.setAttribute('fill', 'none');
      ring.setAttribute('stroke', isTrj ? '#DC2626' : '#3B82F6');
      ring.setAttribute('stroke-width', '2');
      ring.setAttribute('opacity', '0.4');
      g.appendChild(ring);

      const ring2 = document.createElementNS(ns, 'circle');
      ring2.setAttribute('r', '28');
      ring2.setAttribute('fill', 'none');
      ring2.setAttribute('stroke', isTrj ? '#DC2626' : '#3B82F6');
      ring2.setAttribute('stroke-width', '1');
      ring2.setAttribute('opacity', '0.2');
      g.appendChild(ring2);
    }

    // Trojan glow
    if (isTrj) {
      const glow = document.createElementNS(ns, 'circle');
      glow.setAttribute('r', '18');
      glow.setAttribute('fill', 'rgba(220,38,38,0.2)');
      g.appendChild(glow);
    }

    // Main circle
    const circle = document.createElementNS(ns, 'circle');
    circle.setAttribute('r', '16');
    circle.setAttribute('fill', isHl ? (isTrj ? '#7f1d1d' : '#1e3a5f') : '#1a2235');
    circle.setAttribute('stroke', n.color);
    circle.setAttribute('stroke-width', isHl ? '2.5' : '1.5');
    if (isTrj) {
      circle.setAttribute('stroke-width', '2.5');
    }
    g.appendChild(circle);

    // Label
    const text = document.createElementNS(ns, 'text');
    text.setAttribute('text-anchor', 'middle');
    text.setAttribute('dominant-baseline', 'middle');
    text.setAttribute('fill', n.color);
    text.setAttribute('font-size', isTrj ? '7' : '8');
    text.setAttribute('font-weight', '700');
    text.setAttribute('font-family', 'Inter, sans-serif');
    text.textContent = n.label;
    g.appendChild(text);

    // Code badge
    const badge = document.createElementNS(ns, 'text');
    badge.setAttribute('text-anchor', 'middle');
    badge.setAttribute('y', '26');
    badge.setAttribute('fill', '#475569');
    badge.setAttribute('font-size', '7');
    badge.setAttribute('font-family', 'JetBrains Mono, monospace');
    badge.textContent = isTrj ? '⚠' : `[${n.code}]`;
    if (isTrj) { badge.setAttribute('fill', '#DC2626'); }
    g.appendChild(badge);

    // Tooltip events
    g.addEventListener('mouseenter', (e) => showTooltip(e, n));
    g.addEventListener('mouseleave', hideTooltip);

    svg.appendChild(g);
  });
}

function showTooltip(e, node) {
  const tt = document.getElementById('tooltip');
  document.getElementById('tt-name').textContent = node.label;
  document.getElementById('tt-type').textContent = node.isTrojan
    ? '⚠ TROJAN NODE — Malicious logic inserted by attacker'
    : `Type: ${node.type.toUpperCase()} | Encoding: [${node.code}]`;
  tt.style.left = (e.clientX + 14) + 'px';
  tt.style.top  = (e.clientY - 10) + 'px';
  tt.classList.add('show');
}
function hideTooltip() {
  document.getElementById('tooltip').classList.remove('show');
}

// ══════════════════════════════════════
// STEP ENGINE
// ══════════════════════════════════════

let currentStep = -1;
const totalSteps = 6;

const stepData = [
  {
    // Step 0: Verilog → Graph
    layers: [0],
    hlClean: [],
    hlTrojan: [],
    hlEdges: false,
    featClean: [],
    featTrojan: [],
    msgClean:  'The Verilog RTL code of the RS232 circuit is parsed by pyverilog. A Data Flow Graph is automatically extracted — each gate becomes a node, each wire becomes an edge.',
    msgTrojan: 'The same Verilog code, but with a Hardware Trojan secretly inserted by an attacker. Extra nodes appear: a TRIGGER, a COUNTER, and a PAYLOAD — forming the trojan structure.',
    showTrojanChip: false,
  },
  {
    // Step 1: Node Encoding
    layers: [0,1],
    hlClean: ['in1','in2','in3'],
    hlTrojan: ['in1','in2','in3'],
    hlEdges: false,
    featClean: ['fc-in'],
    featTrojan: ['ft-in'],
    msgClean:  'Each node is assigned a number based on its gate type. INPUT nodes get code [1]. AND gets [11], OR gets [14], XOR gets [20], OUTPUT gets [16]. These numbers are the node "features" fed into the GNN.',
    msgTrojan: 'Same encoding for normal nodes. BUT — the TRIGGER, COUNTER, and PAYLOAD trojan nodes have unusual connectivity patterns that don\'t appear in clean circuits. The GNN will learn to spot this!',
    showTrojanChip: false,
  },
  {
    // Step 2: Message Passing Layer 1
    layers: [0,1,2],
    hlClean: ['and1','or1'],
    hlTrojan: ['and1','or1','trg1'],
    hlEdges: true,
    featClean: ['fc-in','fc-and','fc-or'],
    featTrojan: ['ft-in','ft-and','ft-or'],
    msgClean:  'GCN Layer 1 — Message Passing: Each node sends its feature [code] to its neighbors. AND and OR gates receive information from INPUT nodes and update their representation.',
    msgTrojan: 'In Layer 1, the TRIGGER node [⚠] starts receiving messages from IN C. This unusual connection — an input directly feeding a counter-like structure — begins to look suspicious to the GNN.',
    showTrojanChip: true,
  },
  {
    // Step 3: Layer 2
    layers: [0,1,2,3],
    hlClean: ['not1','xor1','and2'],
    hlTrojan: ['not1','xor1','and2','trg2','pay1'],
    hlEdges: true,
    featClean: ['fc-in','fc-and','fc-or','fc-not','fc-xor'],
    featTrojan: ['ft-in','ft-and','ft-or','ft-not','ft-xor'],
    msgClean:  'GCN Layer 2 — Deeper Learning: Each node now knows about nodes 2 hops away. The NOT and XOR gates combine information from AND and OR. The overall pattern is clean and structured.',
    msgTrojan: 'Layer 2 reveals the trojan chain: COUNTER connects to PAYLOAD, which feeds back into the OUTPUT. This creates a structural fingerprint — extra parallel path not present in clean circuits!',
    showTrojanChip: true,
  },
  {
    // Step 4: Pooling
    layers: [0,1,2,3,4],
    hlClean: ['and2','out1'],
    hlTrojan: ['and2','out1','pay1'],
    hlEdges: false,
    featClean: ['fc-in','fc-and','fc-or','fc-not','fc-xor','fc-sig','fc-out'],
    featTrojan: ['ft-in','ft-and','ft-or','ft-not','ft-xor','ft-sig','ft-out'],
    msgClean:  'TopK Pooling: The most informative nodes are selected and their features are combined into a single graph-level vector — a "fingerprint" of this entire circuit. This fingerprint is clean and regular.',
    msgTrojan: 'TopK Pooling selects the most important nodes — and the PAYLOAD node scores high due to its unusual multi-path connectivity. The circuit fingerprint includes the trojan signature!',
    showTrojanChip: true,
  },
  {
    // Step 5: Classification
    layers: [0,1,2,3,4,5],
    hlClean: ['out1'],
    hlTrojan: ['pay1','out1'],
    hlEdges: false,
    featClean: ['fc-in','fc-and','fc-or','fc-not','fc-xor','fc-sig','fc-out'],
    featTrojan: ['ft-in','ft-and','ft-or','ft-not','ft-xor','ft-sig','ft-out'],
    msgClean:  '✅ Classification Complete! The GNN outputs class 0 = NON-TROJAN. The circuit has normal structure: clean signal flow, no anomalous nodes, regular connectivity patterns throughout.',
    msgTrojan: '🚨 TROJAN DETECTED! The GNN outputs class 1 = TROJAN. Extra nodes with unusual connectivity, a parallel data path to the output, and counter-based trigger logic — all signs of Hardware Trojan!',
    showTrojanChip: true,
    showResult: true,
  },
];

function goStep(stepIndex) {
  currentStep = stepIndex;
  applyStep(stepIndex);
}

function nextStep() {
  if (currentStep < totalSteps - 1) {
    currentStep++;
    applyStep(currentStep);
  }
  if (currentStep === totalSteps - 1) {
    document.getElementById('nextBtn').textContent = '↺ Restart';
    document.getElementById('nextBtn').onclick = () => { resetAll(); };
  }
}

function prevStep() {
  if (currentStep > 0) {
    currentStep--;
    applyStep(currentStep);
  }
  document.getElementById('nextBtn').textContent = 'Next Step →';
  document.getElementById('nextBtn').onclick = nextStep;
}

function resetAll() {
  currentStep = -1;
  document.getElementById('nextBtn').textContent = 'Next Step →';
  document.getElementById('nextBtn').onclick = nextStep;
  drawCircuit('svgClean',  cleanNodes,  cleanEdges,  [], false);
  drawCircuit('svgTrojan', trojanNodes, trojanEdges, [], false);
  // Reset layers
  for (let i=0;i<6;i++) {
    document.getElementById('lc'+i).classList.remove('lit');
    document.getElementById('lt'+i).classList.remove('lit');
  }
  // Reset feats
  document.querySelectorAll('.feat-chip').forEach(c => c.classList.remove('active'));
  document.getElementById('ft-trj').style.display = 'none';
  // Reset messages
  document.getElementById('msgCleanText').textContent = 'Press "Next Step" to begin the analysis...';
  document.getElementById('msgTrojanText').textContent = 'Press "Next Step" to begin the analysis...';
  // Reset results
  document.getElementById('resultClean').classList.remove('visible');
  document.getElementById('resultTrojan').classList.remove('visible');
  // Reset steps bar
  document.querySelectorAll('.step-pill').forEach((p,i) => p.classList.remove('active'));
  document.getElementById('progressBar').style.width = '0%';
}

function applyStep(idx) {
  const s = stepData[idx];
  if (!s) return;

  // Progress bar
  document.getElementById('progressBar').style.width = ((idx+1)/totalSteps*100) + '%';

  // Step pills
  document.querySelectorAll('.step-pill').forEach((p,i) => {
    p.classList.toggle('active', i === idx);
  });

  // Draw circuits
  drawCircuit('svgClean',  cleanNodes,  cleanEdges,  s.hlClean,  s.hlEdges);
  drawCircuit('svgTrojan', trojanNodes, trojanEdges, s.hlTrojan, s.hlEdges);

  // Layers
  for (let i=0;i<6;i++) {
    document.getElementById('lc'+i).classList.toggle('lit', s.layers.includes(i));
    document.getElementById('lt'+i).classList.toggle('lit', s.layers.includes(i));
  }

  // Node features
  document.querySelectorAll('.feat-chip').forEach(c => c.classList.remove('active'));
  s.featClean.forEach(id => { const el=document.getElementById(id); if(el) el.classList.add('active'); });
  s.featTrojan.forEach(id => { const el=document.getElementById(id); if(el) el.classList.add('active'); });
  const trjChip = document.getElementById('ft-trj');
  if (s.showTrojanChip) {
    trjChip.style.display = 'inline-block';
    trjChip.classList.add('active');
  } else {
    trjChip.style.display = 'none';
  }

  // Messages
  document.getElementById('msgCleanText').textContent  = s.msgClean;
  document.getElementById('msgTrojanText').textContent = s.msgTrojan;

  // Results
  if (s.showResult) {
    setTimeout(() => {
      document.getElementById('resultClean').classList.add('visible');
      document.getElementById('resultTrojan').classList.add('visible');
    }, 400);
  } else {
    document.getElementById('resultClean').classList.remove('visible');
    document.getElementById('resultTrojan').classList.remove('visible');
  }
}

// ── INIT ──
drawCircuit('svgClean',  cleanNodes,  cleanEdges,  [], false);
drawCircuit('svgTrojan', trojanNodes, trojanEdges, [], false);
</script>
</body>
</html>


==========================================================

## Metrics 

[metrics_slide.html](https://github.com/user-attachments/files/31804598/metrics_slide.html)


========================================================

## Some Graph Visualized:

<img width="7170" height="4765" alt="comparison_small_graphs" src="https://github.com/user-attachments/assets/9edf8d57-1f73-45fe-a6d5-2f84cd9111cd" />

============================================================

<img width="6060" height="5011" alt="graph_1_det_1011_TjFree" src="https://github.com/user-attachments/assets/594cab81-5cef-4e95-970b-6f3202894f47" />

============================================================

<img width="6060" height="5011" alt="graph_3_spi_master_TjFree" src="https://github.com/user-attachments/assets/93352dec-8a42-4ead-aea6-fbba164c8d4c" />

=============================================================

<img width="1500" height="1240" alt="graph_5_RS232-T900_TjI" src="https://github.com/user-attachments/assets/c4505165-ea93-4c5e-bee4-486f7c433d00" />

=============================================================

<img width="1500" height="1240" alt="graph_4_RS232-T600_TjI" src="https://github.com/user-attachments/assets/9b62d0cd-8267-413e-b11a-6fce5bdd3f2c" />

=============================================================

<img width="1500" height="1240" alt="graph_2_PIC16F84-T200_TjI" src="https://github.com/user-attachments/assets/6c8bfbbb-4cab-458c-9a27-7d07f6a85735" />

=============================================================





