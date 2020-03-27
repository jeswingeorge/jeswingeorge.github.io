I tried to implement the Tableau visualizations using LOD expressions made in this [blog](https://www.tableau.com/about/blog/LOD-expressions).



1. Customer order frequency

Number of customers who ordered one order, two orders, three others and four others. To create a histogram of the number of customers who purchased 1,2,3,...N times.  
Tips: Measures are aggregated when added to the view. Adding a dimension will increase the granularity of the view.  

Steps:
- Create a calculated field __Number of distinct customers__: `COUNTD([Customer ID])` and drag it to Rows shelf and set Marks card to Bar type. 
- Create a calculated field __Number of orders per customer__: `{ FIXED [Customer ID]: COUNTD([Order ID])}` and convert this to dimension.
- Then drag the calculated field __Number of orders per customer__ to the Column shelf and also drag calculated field __Number of distinct customers__ to the color and Label Marks card. Then we get a histogram of the number of customers who purchased 1,2,3,...N times.
- Then do the necessary formatting, title and tooltip modififcation.

Link to the created visualisation.](https://public.tableau.com/views/HistogramofCustomerOrdersCount/Dashboard1?:display_count=y&publish=yes&:origin=viz_share_link)

<div class='tableauPlaceholder' id='viz1585334395608' style='position: relative'><noscript><a href='#'><img alt=' ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Hi&#47;HistogramofCustomerOrdersCount&#47;Dashboard1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='HistogramofCustomerOrdersCount&#47;Dashboard1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Hi&#47;HistogramofCustomerOrdersCount&#47;Dashboard1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='filter' value='publish=yes' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1585334395608');                    var vizElement = divElement.getElementsByTagName('object')[0];                    if ( divElement.offsetWidth > 800 ) { vizElement.style.minWidth='420px';vizElement.style.maxWidth='750px';vizElement.style.width='100%';vizElement.style.minHeight='587px';vizElement.style.maxHeight='887px';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';} else if ( divElement.offsetWidth > 500 ) { vizElement.style.minWidth='420px';vizElement.style.maxWidth='750px';vizElement.style.width='100%';vizElement.style.minHeight='587px';vizElement.style.maxHeight='887px';vizElement.style.height=(divElement.offsetWidth*0.75)+'px';} else { vizElement.style.width='100%';vizElement.style.height='727px';}                     var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>


![LOD1](../../images/tableau/15_lods/load1.PNG)
