<!-- ``` {r image, out.width = "200px", echo = FALSE} -->
<!-- knitr::include_graphics("Taylor_Swift_Cavendish.jpg") -->
<!-- ``` -->

# A Topic Modelling Analysis of Taylor Swift Song Lyrics 🎤

I was browsing Kaggle for an interesting dataset when I ran across a
[library of Taylor Swift’s
songs](https://www.kaggle.com/PromptCloudHQ/taylor-swift-song-lyrics-from-all-the-albums).

Usually when I find data on Kaggle, it’s been picked over and analyzed
all sort of ways. But luckily for me, only one person had analyzed these
data! I guess data scientists aren’t much into pop culture (or data not
associated with million-dollar competitions).

I thought it might be fun to look for patterns in Taylor’s music. My
running theory for the past decade is that Taylor’s songs all sound the
same. Would the data bear that pattern out?

Speficially, this analysis seeks to answer the following questions:

1.  Did the length of Taylor’s songs change over time?
2.  Did the complexity of songs change over time?
3.  Do Taylor’s songs tend to coalesce around a few topics? If so, how
    might we charcterize those topics?
4.  Does a trend exist between song topic and song year released?

Before we dive into the analysis, let’s run a few quality checks on the
data.

1.  How many records do we have here?

<!-- -->

    ## [1] 7

1.  Is Taylor Swift the only artist represented in the data?

<!-- -->

    ## [1] "Taylor Swift"

Oh good, yes she is.

1.  Which songs are in the data?

<!-- -->

    ##  [1] "Tim McGraw"                             
    ##  [2] "Picture To Burn"                        
    ##  [3] "Teardrops On My Guitar"                 
    ##  [4] "A Place In This World"                  
    ##  [5] "Cold as You"                            
    ##  [6] "The Outside"                            
    ##  [7] "Tied Together With A Smile"             
    ##  [8] "Stay Beautiful"                         
    ##  [9] "Should've Said No"                      
    ## [10] "Mary's Song (Oh My My My)"              
    ## [11] "Our Song"                               
    ## [12] "I'm Only Me When I'm With You"          
    ## [13] "Invisible"                              
    ## [14] "A Perfectly Good Heart"                 
    ## [15] "Fearless"                               
    ## [16] "Fifteen"                                
    ## [17] "Love Story"                             
    ## [18] "Hey Stephen"                            
    ## [19] "White Horse"                            
    ## [20] "You Belong With Me"                     
    ## [21] "Breathe (Ft. Colbie Caillat)"           
    ## [22] "Tell Me Why"                            
    ## [23] "You're Not Sorry"                       
    ## [24] "The Way I Loved You"                    
    ## [25] "Forever & Always"                       
    ## [26] "The Best Day"                           
    ## [27] "Change"                                 
    ## [28] "Mine"                                   
    ## [29] "Sparks Fly"                             
    ## [30] "Back To December"                       
    ## [31] "Speak Now"                              
    ## [32] "Dear John"                              
    ## [33] "Mean"                                   
    ## [34] "The Story of Us"                        
    ## [35] "Never Grow Up"                          
    ## [36] "Enchanted"                              
    ## [37] "Better Than Revenge"                    
    ## [38] "Innocent"                               
    ## [39] "Haunted"                                
    ## [40] "Last Kiss"                              
    ## [41] "Long Live"                              
    ## [42] "Ours"                                   
    ## [43] "If This Was a Movie"                    
    ## [44] "Superman"                               
    ## [45] "State of Grace"                         
    ## [46] "Red"                                    
    ## [47] "Treacherous"                            
    ## [48] "I Knew You Were Trouble"                
    ## [49] "All Too Well"                           
    ## [50] "22"                                     
    ## [51] "I Almost Do"                            
    ## [52] "We Are Never Ever Getting Back Together"
    ## [53] "Stay Stay Stay"                         
    ## [54] "The Last Time (Ft. Gary Lightbody)"     
    ## [55] "Holy Ground"                            
    ## [56] "Sad Beautiful Tragic"                   
    ## [57] "The Lucky One"                          
    ## [58] "Everything Has Changed (Ft. Ed Sheeran)"
    ## [59] "Starlight"                              
    ## [60] "Begin Again"                            
    ## [61] "The Moment I Knew"                      
    ## [62] "Come Back... Be Here"                   
    ## [63] "Girl at Home"                           
    ## [64] "Welcome to New York"                    
    ## [65] "Blank Space"                            
    ## [66] "Style"                                  
    ## [67] "Out of the Woods"                       
    ## [68] "All You Had to Do Was Stay"             
    ## [69] "Shake It Off"                           
    ## [70] "I Wish You Would"                       
    ## [71] "Bad Blood"                              
    ## [72] "Wildest Dreams"                         
    ## [73] "How You Get The Girl"                   
    ## [74] "This Love"                              
    ## [75] "I Know Places"                          
    ## [76] "Clean"                                  
    ## [77] "Wonderland"                             
    ## [78] "You Are in Love"                        
    ## [79] "New Romantics"                          
    ## [80] "...Ready for It?"                       
    ## [81] "End Game (Ft. Ed Sheeran & Future)"     
    ## [82] "I Did Something Bad"                    
    ## [83] "Don't Blame Me"                         
    ## [84] "Delicate"                               
    ## [85] "Look What You Made Me Do"               
    ## [86] "So It Goes..."                          
    ## [87] "Gorgeous"                               
    ## [88] "Getaway Car"                            
    ## [89] "King of My Heart"                       
    ## [90] "Dancing With Our Hands Tied"            
    ## [91] "Dress"                                  
    ## [92] "This Is Why We Can't Have Nice Things"  
    ## [93] "Call It What You Want"                  
    ## [94] "New Year's Day"

The data needs pre-processing, which includes removing stopwords and
special characters and coercing every word to lowercase. Removing
stopwords is a typical natural language processing task, but it gives me
some anxiety because some of Taylor’s most prominent songs (such as “You
Belong With Me”) contain stopwords that impact song meaning (“You”,
“With”, “Me”).

In the original dataset, every row contains one song lyric, which is not
ideal from a data tidyness point of view. The steps are hidden from
view, but I end up with a reshaped dataset.

    ## Joining, by = "word"

## Question 1: Visualizing song length over time

I’m interpreting song length as the number of unique tokens in a song.
Below, I plot the trend of average song length over the years.

    ## `summarise()` has grouped output by 'track_title'. You can override using the `.groups` argument.

<div id="htmlwidget-32a48a59aafc8d2116bc" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-32a48a59aafc8d2116bc">{"x":{"hc_opts":{"chart":{"reflow":true},"title":{"text":"Average Song Word Count by Year"},"yAxis":{"title":{"text":null}},"credits":{"enabled":false},"exporting":{"enabled":false},"boost":{"enabled":false},"plotOptions":{"series":{"label":{"enabled":false},"turboThreshold":0},"treemap":{"layoutAlgorithm":"squarified"}},"xAxis":{"categories":[2006,2008,2010,2012,2014,2017]},"series":[{"data":[70.7142857142857,82.6923076923077,103,90.0526315789474,119.9375,132.133333333333],"name":"Word Count","color":"#815BFF"}]},"theme":{"colors":["#f1c40f","#2ecc71","#9b59b6","#e74c3c","#34495e","#3498db","#1abc9c","#f39c12","#d35400"],"chart":{"backgroundColor":"#ECF0F1"},"xAxis":{"gridLineDashStyle":"Dash","gridLineWidth":1,"gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"yAxis":{"gridLineDashStyle":"Dash","gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"legendBackgroundColor":"rgba(0, 0, 0, 0.5)","background2":"#505053","dataLabelsColor":"#B0B0B3","textColor":"#34495e","contrastTextColor":"#F0F0F3","maskColor":"rgba(255,255,255,0.3)"},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script>

The graph above shows that the average word count in Taylor’s songs have
increased rather steadily over the years.

This chart does not account for within-year variation. It’s entirely
possible that a few songs had high word count in the later years of
Taylor’s career, which brought the average up. Let’s look at that
within-year variation.

<div id="htmlwidget-e4021586ad58ad6a55ef" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-e4021586ad58ad6a55ef">{"x":{"hc_opts":{"chart":{"reflow":true,"type":"column"},"title":{"text":"Distribution of Song Word Count by Year"},"yAxis":{"title":{"text":null}},"credits":{"enabled":false},"exporting":{"enabled":false},"boost":{"enabled":false},"plotOptions":{"series":{"label":{"enabled":false},"turboThreshold":0,"marker":{"symbol":"circle"},"showInLegend":false},"treemap":{"layoutAlgorithm":"squarified"}},"xAxis":{"type":"category","categories":""},"series":[{"name":null,"data":[{"name":2006,"low":21,"q1":57,"median":66.5,"q3":84,"high":120},{"name":2008,"low":55,"q1":74,"median":83,"q3":92,"high":119},{"name":2010,"low":83,"q1":100,"median":107,"q3":114,"high":123},{"name":2012,"low":54,"q1":73,"median":90,"q3":108,"high":126},{"name":2014,"low":78,"q1":88.5,"median":99,"q3":138.5,"high":186},{"name":2017,"low":86,"q1":91,"median":119,"q3":161.5,"high":237}],"type":"boxplot","id":null,"color":"#815BFF"}]},"theme":{"colors":["#f1c40f","#2ecc71","#9b59b6","#e74c3c","#34495e","#3498db","#1abc9c","#f39c12","#d35400"],"chart":{"backgroundColor":"#ECF0F1"},"xAxis":{"gridLineDashStyle":"Dash","gridLineWidth":1,"gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"yAxis":{"gridLineDashStyle":"Dash","gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"legendBackgroundColor":"rgba(0, 0, 0, 0.5)","background2":"#505053","dataLabelsColor":"#B0B0B3","textColor":"#34495e","contrastTextColor":"#F0F0F3","maskColor":"rgba(255,255,255,0.3)"},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script>

The distribution of word counts by song becomes more spread out over the
years. Interestingly, small word-count songs get larger as time goes on
as well, which suggests that the widening distributions are driven by
greater wordiness and not a greater spread over word count length.
Taylor released her wordiest songs in 2017. Is this trend a sign that
Taylor’s songs are getting more intricate or complex over time?

## Question 2: Visualizing song complexity over time

I define “song conplexity” as the number of unique non-stopword words
per song. I look at song complexity from two differnet angles:

1.  On average, how many unique words appear per song per year?
2.  On average, how many times is a word repeated per song?

<!-- -->

    ## Joining, by = "word"

    ## `summarise()` has grouped output by 'track_title'. You can override using the `.groups` argument.

<div id="htmlwidget-4981f04cb2e66b63175c" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-4981f04cb2e66b63175c">{"x":{"hc_opts":{"chart":{"reflow":true},"title":{"text":"Unique Words in Songs by Year"},"yAxis":{"title":{"text":null}},"credits":{"enabled":false},"exporting":{"enabled":false},"boost":{"enabled":false},"plotOptions":{"series":{"label":{"enabled":false},"turboThreshold":0},"treemap":{"layoutAlgorithm":"squarified"}},"xAxis":{"categories":[2006,2008,2010,2012,2014,2017]},"series":[{"data":[37.1428571428571,50.9230769230769,60.8823529411765,47.4210526315789,48.5625,57.5333333333333],"name":"Average Number of Unique Words","color":"#815BFF"}]},"theme":{"colors":["#f1c40f","#2ecc71","#9b59b6","#e74c3c","#34495e","#3498db","#1abc9c","#f39c12","#d35400"],"chart":{"backgroundColor":"#ECF0F1"},"xAxis":{"gridLineDashStyle":"Dash","gridLineWidth":1,"gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"yAxis":{"gridLineDashStyle":"Dash","gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"legendBackgroundColor":"rgba(0, 0, 0, 0.5)","background2":"#505053","dataLabelsColor":"#B0B0B3","textColor":"#34495e","contrastTextColor":"#F0F0F3","maskColor":"rgba(255,255,255,0.3)"},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script>

The chart above shows that Taylor reached her greatest lexical
complecity in 2010. 2010 roughly marks Taylor’s transition from a
country music artist to a pop artist. The most notable trend is that as
Taylor’s songs skewed more heavily toward the pop music genre, the
number of unique words in songs decreased. But again, looking at
averages doesn’t get us very far, so let’s take a gander at boxplots.

<div id="htmlwidget-e178c439bd22fe4f1bb5" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-e178c439bd22fe4f1bb5">{"x":{"hc_opts":{"chart":{"reflow":true,"type":"column"},"title":{"text":"Distribution of Unique Words by Year"},"yAxis":{"title":{"text":null}},"credits":{"enabled":false},"exporting":{"enabled":false},"boost":{"enabled":false},"plotOptions":{"series":{"label":{"enabled":false},"turboThreshold":0,"marker":{"symbol":"circle"},"showInLegend":false},"treemap":{"layoutAlgorithm":"squarified"}},"xAxis":{"type":"category","categories":""},"series":[{"name":null,"data":[{"name":2006,"low":14,"q1":26,"median":37.5,"q3":44,"high":63},{"name":2008,"low":34,"q1":42,"median":49,"q3":58,"high":67},{"name":2010,"low":45,"q1":56,"median":60,"q3":68,"high":85},{"name":2012,"low":29,"q1":40.5,"median":44,"q3":53,"high":57},{"name":2014,"low":26,"q1":42,"median":47.5,"q3":54,"high":62},{"name":2017,"low":43,"q1":48,"median":51,"q3":60.5,"high":76}],"type":"boxplot","id":null,"color":"#815BFF"}]},"theme":{"colors":["#f1c40f","#2ecc71","#9b59b6","#e74c3c","#34495e","#3498db","#1abc9c","#f39c12","#d35400"],"chart":{"backgroundColor":"#ECF0F1"},"xAxis":{"gridLineDashStyle":"Dash","gridLineWidth":1,"gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"yAxis":{"gridLineDashStyle":"Dash","gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"legendBackgroundColor":"rgba(0, 0, 0, 0.5)","background2":"#505053","dataLabelsColor":"#B0B0B3","textColor":"#34495e","contrastTextColor":"#F0F0F3","maskColor":"rgba(255,255,255,0.3)"},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script>

The distribution of unique words in songs over the years is quite
similar, but gets tighter over time.

    ## Joining, by = "word"

    ## `summarise()` has grouped output by 'track_title'. You can override using the `.groups` argument.

<div id="htmlwidget-707e389d6e72b665729c" style="width:100%;height:500px;" class="highchart html-widget"></div>
<script type="application/json" data-for="htmlwidget-707e389d6e72b665729c">{"x":{"hc_opts":{"chart":{"reflow":true},"title":{"text":"Average Word Repetition by Year"},"yAxis":{"title":{"text":null}},"credits":{"enabled":false},"exporting":{"enabled":false},"boost":{"enabled":false},"plotOptions":{"series":{"label":{"enabled":false},"turboThreshold":0},"treemap":{"layoutAlgorithm":"squarified"}},"xAxis":{"categories":[2006,2008,2010,2012,2014,2017]},"series":[{"data":[1.94895118269785,1.66106157656639,1.71013164602256,1.99234604197424,2.57458285445842,2.30508502645926],"name":"Average Number of Word Repetitions","color":"#815BFF"}]},"theme":{"colors":["#f1c40f","#2ecc71","#9b59b6","#e74c3c","#34495e","#3498db","#1abc9c","#f39c12","#d35400"],"chart":{"backgroundColor":"#ECF0F1"},"xAxis":{"gridLineDashStyle":"Dash","gridLineWidth":1,"gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"yAxis":{"gridLineDashStyle":"Dash","gridLineColor":"#BDC3C7","lineColor":"#BDC3C7","minorGridLineColor":"#BDC3C7","tickColor":"#BDC3C7","tickWidth":1},"legendBackgroundColor":"rgba(0, 0, 0, 0.5)","background2":"#505053","dataLabelsColor":"#B0B0B3","textColor":"#34495e","contrastTextColor":"#F0F0F3","maskColor":"rgba(255,255,255,0.3)"},"conf_opts":{"global":{"Date":null,"VMLRadialGradientURL":"http =//code.highcharts.com/list(version)/gfx/vml-radial-gradient.png","canvasToolsURL":"http =//code.highcharts.com/list(version)/modules/canvas-tools.js","getTimezoneOffset":null,"timezoneOffset":0,"useUTC":true},"lang":{"contextButtonTitle":"Chart context menu","decimalPoint":".","downloadJPEG":"Download JPEG image","downloadPDF":"Download PDF document","downloadPNG":"Download PNG image","downloadSVG":"Download SVG vector image","drillUpText":"Back to {series.name}","invalidDate":null,"loading":"Loading...","months":["January","February","March","April","May","June","July","August","September","October","November","December"],"noData":"No data to display","numericSymbols":["k","M","G","T","P","E"],"printChart":"Print chart","resetZoom":"Reset zoom","resetZoomTitle":"Reset zoom level 1:1","shortMonths":["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"],"thousandsSep":" ","weekdays":["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]}},"type":"chart","fonts":[],"debug":false},"evals":[],"jsHooks":[]}</script>

Word repetition increased dramatically after 2010, and was at its
highest in 2014. In 2014, Taylor repeated words on average 2.57 times.

## Question 3: What topics are typically covered in Taylor’s songs?

Does Taylor Swift really sing about the same topics, or am I salty? To
find out, I use a method called [Latent Dirichlet Allocation
(LDA)](https://eight2late.wordpress.com/2015/09/29/a-gentle-introduction-to-topic-modeling-using-r/).
LDA is a generative probabilistic model commonly used to model topics in
documents.

LDA assumes that each document is a mixture of topics, and each topic is
a mixture of words. It can be used for many different types of analyses.
The probabilistic model generated by LDA assigns probabilities to a word
falling under a given topic, and to a topic being assigned to a given
document.

I will treat each song as a “document.” Before we dive into the
analysis, let’s look at the most frequent words in her collection.

    datV2 %>% 
      unnest_tokens(word, text) %>%
      anti_join(stop_words) %>% 
      count(word, sort = TRUE)

    ## Joining, by = "word"

    ##             word   n
    ## 1           time 164
    ## 2           love 160
    ## 3           baby 115
    ## 4            ooh 103
    ## 5           stay  88
    ## 6          wanna  82
    ## 7          shake  80
    ## 8           yeah  80
    ## 9             ey  72
    ## 10         night  72
    ## 11          girl  66
    ## 12          feel  64
    ## 13           bad  63
    ## 14         gonna  62
    ## 15          call  60
    ## 16          home  60
    ## 17          eyes  52
    ## 18       dancing  51
    ## 19         break  49
    ## 20          life  47
    ## 21          mind  46
    ## 22       forever  45
    ## 23       waiting  45
    ## 24            ah  44
    ## 25      remember  44
    ## 26            la  42
    ## 27        lights  42
    ## 28         hands  41
    ## 29           day  40
    ## 30            ha  39
    ## 31          hold  39
    ## 32     beautiful  38
    ## 33         woods  38
    ## 34         heart  37
    ## 35           car  35
    ## 36           hey  34
    ## 37          whoa  34
    ## 38       feeling  33
    ## 39         dress  32
    ## 40          play  31
    ## 41          door  30
    ## 42          hate  30
    ## 43           mad  30
    ## 44           run  30
    ## 45          york  30
    ## 46          hope  29
    ## 47        dreams  28
    ## 48         light  28
    ## 49         world  28
    ## 50          game  27
    ## 51        forget  26
    ## 52       friends  26
    ## 53          live  26
    ## 54          lost  26
    ## 55        people  26
    ## 56         smile  26
    ## 57          talk  26
    ## 58          fall  25
    ## 59           fly  25
    ## 60          head  25
    ## 61          kiss  25
    ## 62          late  25
    ## 63         leave  25
    ## 64          mine  25
    ## 65            uh  25
    ## 66         wrong  25
    ## 67        coming  24
    ## 68          rain  24
    ## 69          song  24
    ## 70       trouble  24
    ## 71          hear  23
    ## 72           met  23
    ## 73     should've  23
    ## 74       getaway  22
    ## 75          miss  22
    ## 76           mmm  22
    ## 77         phone  22
    ## 78           red  22
    ## 79     starlight  22
    ## 80         dance  21
    ## 81          fake  21
    ## 82        follow  21
    ## 83         found  21
    ## 84          grow  21
    ## 85          meet  21
    ## 86          nice  21
    ## 87          save  21
    ## 88      thinking  21
    ## 89    wonderland  21
    ## 90        change  20
    ## 91          hand  20
    ## 92          hard  20
    ## 93         loved  20
    ## 94          tied  20
    ## 95         heard  19
    ## 96         makes  19
    ## 97            mm  19
    ## 98       someday  19
    ## 99          town  19
    ## 100         wait  19
    ## 101         walk  19
    ## 102        drive  18
    ## 103         left  18
    ## 104      silence  18
    ## 105         beat  17
    ## 106        crazy  17
    ## 107      darling  17
    ## 108          eye  17
    ## 109        shine  17
    ## 110     standing  17
    ## 111        watch  17
    ## 112        begin  16
    ## 113          bet  16
    ## 114        blood  16
    ## 115         blue  16
    ## 116         dark  16
    ## 117     delicate  16
    ## 118        honey  16
    ## 119        lucky  16
    ## 120        story  16
    ## 121         burn  15
    ## 122         deep  15
    ## 123        fight  15
    ## 124        games  15
    ## 125       ground  15
    ## 126         lips  15
    ## 127         list  15
    ## 128          sad  15
    ## 129        stood  15
    ## 130      alright  14
    ## 131         cold  14
    ## 132         cool  14
    ## 133        girls  14
    ## 134     memories  14
    ## 135       moment  14
    ## 136        stand  14
    ## 137         true  14
    ## 138      wishing  14
    ## 139           22  13
    ## 140          aah  13
    ## 141       belong  13
    ## 142         easy  13
    ## 143        front  13
    ## 144     gorgeous  13
    ## 145        gotta  13
    ## 146     laughing  13
    ## 147         lose  13
    ## 148        magic  13
    ## 149      perfect  13
    ## 150       pretty  13
    ## 151       simple  13
    ## 152         team  13
    ## 153      tonight  13
    ## 154       walked  13
    ## 155      anymore  12
    ## 156      breathe  12
    ## 157      changed  12
    ## 158         days  12
    ## 159     december  12
    ## 160      finally  12
    ## 161         fine  12
    ## 162         free  12
    ## 163         hair  12
    ## 164       loving  12
    ## 165    perfectly  12
    ## 166        rains  12
    ## 167   reputation  12
    ## 168         rest  12
    ## 169        round  12
    ## 170         soul  12
    ## 171       string  12
    ## 172        times  12
    ## 173        touch  12
    ## 174         usin  12
    ## 175      walking  12
    ## 176        worse  12
    ## 177        black  11
    ## 178         body  11
    ## 179       bought  11
    ## 180         city  11
    ## 181     daydream  11
    ## 182        feels  11
    ## 183         held  11
    ## 184        laugh  11
    ## 185       middle  11
    ## 186       nights  11
    ## 187        shame  11
    ## 188      shoulda  11
    ## 189         sing  11
    ## 190       starts  11
    ## 191        style  11
    ## 192         till  11
    ## 193        tired  11
    ## 194        walls  11
    ## 195        white  11
    ## 196       window  11
    ## 197        words  11
    ## 198         bout  10
    ## 199         busy  10
    ## 200         dead  10
    ## 201         doin  10
    ## 202        dream  10
    ## 203         feet  10
    ## 204        floor  10
    ## 205       friend  10
    ## 206        funny  10
    ## 207          haa  10
    ## 208        happy  10
    ## 209       insane  10
    ## 210         lord  10
    ## 211        lying  10
    ## 212      missing  10
    ## 213       months  10
    ## 214      morning  10
    ## 215      picture  10
    ## 216         read  10
    ## 217         road  10
    ## 218    screaming  10
    ## 219       single  10
    ## 220       street  10
    ## 221        trust  10
    ## 222         babe   9
    ## 223        blame   9
    ## 224          boy   9
    ## 225       caught   9
    ## 226        clean   9
    ## 227     could've   9
    ## 228      dressed   9
    ## 229         drug   9
    ## 230           em   9
    ## 231     fearless   9
    ## 232         fell   9
    ## 233      fifteen   9
    ## 234      figured   9
    ## 235          fun   9
    ## 236          god   9
    ## 237      goodbye   9
    ## 238         hide   9
    ## 239      killing   9
    ## 240         lead   9
    ## 241       losing   9
    ## 242        moved   9
    ## 243         pain   9
    ## 244        party   9
    ## 245         past   9
    ## 246     pictures   9
    ## 247         real   9
    ## 248      shaking   9
    ## 249      sitting   9
    ## 250       sparks   9
    ## 251        speak   9
    ## 252        throw   9
    ## 253  treacherous   9
    ## 254         wild   9
    ## 255      wildest   9
    ## 256         boys   8
    ## 257       breath   8
    ## 258       bright   8
    ## 259        broke   8
    ## 260      burning   8
    ## 261       chance   8
    ## 262       cheeks   8
    ## 263        close   8
    ## 264          cut   8
    ## 265     favorite   8
    ## 266     fighting   8
    ## 267        green   8
    ## 268        guess   8
    ## 269          hit   8
    ## 270           ho   8
    ## 271         hurt   8
    ## 272     innocent   8
    ## 273       inside   8
    ## 274         king   8
    ## 275       looked   8
    ## 276          lot   8
    ## 277         page   8
    ## 278      pouring   8
    ## 279      pretend   8
    ## 280         slow   8
    ## 281         stop   8
    ## 282       taking   8
    ## 283      talking   8
    ## 284         told   8
    ## 285       undone   8
    ## 286      watched   8
    ## 287        water   8
    ## 288         word   8
    ## 289        worth   8
    ## 290            2   7
    ## 291      actress   7
    ## 292        bring   7
    ## 293        cried   7
    ## 294          cry   7
    ## 295     dreaming   7
    ## 296     drowning   7
    ## 297       flames   7
    ## 298    flashback   7
    ## 299         flew   7
    ## 300       golden   7
    ## 301         half   7
    ## 302   hallelujah   7
    ## 303   heartbreak   7
    ## 304        jeans   7
    ## 305         line   7
    ## 306       lonely   7
    ## 307     mistakes   7
    ## 308        movie   7
    ## 309       pieces   7
    ## 310        ready   7
    ## 311     realized   7
    ## 312       reason   7
    ## 313      running   7
    ## 314         scar   7
    ## 315        scene   7
    ## 316         sick   7
    ## 317      singing   7
    ## 318          sky   7
    ## 319        storm   7
    ## 320       stupid   7
    ## 321     superman   7
    ## 322         wear   7
    ## 323    wondering   7
    ## 324        angel   6
    ## 325       baby's   6
    ## 326      brought   6
    ## 327       burned   6
    ## 328        catch   6
    ## 329      clothes   6
    ## 330 conversation   6
    ## 331     counting   6
    ## 332       crying   6
    ## 333         drop   6
    ## 334    enchanted   6
    ## 335      falling   6
    ## 336       faster   6
    ## 337         fate   6
    ## 338         fore   6
    ## 339       future   6
    ## 340        grown   6
    ## 341      holding   6
    ## 342     honestly   6
    ## 343        house   6
    ## 344    invisible   6
    ## 345      kingdom   6
    ## 346     lipstick   6
    ## 347       lovers   6
    ## 348        loves   6
    ## 349        lovin   6
    ## 350          low   6
    ## 351       mcgraw   6
    ## 352         mess   6
    ## 353         move   6
    ## 354         pick   6
    ## 355      players   6
    ## 356     reckless   6
    ## 357        rocks   6
    ## 358    romantics   6
    ## 359        romeo   6
    ## 360       scared   6
    ## 361       shined   6
    ## 362        slope   6
    ## 363        solve   6
    ## 364        space   6
    ## 365        spend   6
    ## 366        stars   6
    ## 367      strange   6
    ## 368       summer   6
    ## 369        threw   6
    ## 370          tim   6
    ## 371        truck   6
    ## 372   understand   6
    ## 373         view   6
    ## 374      water's   6
    ## 375      wearing   6
    ## 376        worst   6
    ## 377       affair   5
    ## 378         arms   5
    ## 379       battle   5
    ## 380          bed   5
    ## 381      begging   5
    ## 382       bigger   5
    ## 383        brand   5
    ## 384     breaking   5
    ## 385       called   5
    ## 386         care   5
    ## 387      careful   5
    ## 388     careless   5
    ## 389       castle   5
    ## 390      chasing   5
    ## 391        chill   5
    ## 392     crashing   5
    ## 393        crowd   5
    ## 394      crowded   5
    ## 395         damn   5
    ## 396     daughter   5
    ## 397         died   5
    ## 398         drag   5
    ## 399        drama   5
    ## 400        dying   5
    ## 401        fears   5
    ## 402      flowers   5
    ## 403       flying   5
    ## 404  forgiveness   5
    ## 405   headlights   5
    ## 406        horse   5
    ## 407         hung   5
    ## 408   impossible   5
    ## 409         joke   5
    ## 410         lies   5
    ## 411       living   5
    ## 412         lock   5
    ## 413        man's   5
    ## 414        match   5
    ## 415       matter   5
    ## 416       messed   5
    ## 417           na   5
    ## 418        names   5
    ## 419   pretenders   5
    ## 420     princess   5
    ## 421        queen   5
    ## 422        radio   5
    ## 423      reasons   5
    ## 424      revenge   5
    ## 425        ridin   5
    ## 426         rush   5
    ## 427       school   5
    ## 428        shirt   5
    ## 429       smiles   5
    ## 430     spinning   5
    ## 431       stairs   5
    ## 432        start   5
    ## 433     stealing   5
    ## 434         step   5
    ## 435      stephen   5
    ## 436     straight   5
    ## 437       strike   5
    ## 438       strong   5
    ## 439       sunset   5
    ## 440        takes   5
    ## 441        talks   5
    ## 442         tall   5
    ## 443       taylor   5
    ## 444        tears   5
    ## 445      telling   5
    ## 446        tight   5
    ## 447          top   5
    ## 448      tragedy   5
    ## 449       tragic   5
    ## 450        twist   5
    ## 451          vow   5
    ## 452         wake   5
    ## 453        why'd   5
    ## 454         wide   5
    ## 455 wonderstruck   5
    ## 456        write   5
    ## 457    yesterday   5
    ## 458       afraid   4
    ## 459          ahh   4
    ## 460        alive   4
    ## 461          bar   4
    ## 462      bedroom   4
    ## 463     believed   4
    ## 464        blind   4
    ## 465        blink   4
    ## 466        block   4
    ## 467       bricks   4
    ## 468        build   4
    ## 469      buttons   4
    ## 470         cafe   4
    ## 471      calling   4
    ## 472        chain   4
    ## 473        check   4
    ## 474       church   4
    ## 475     confused   4
    ## 476      crashed   4
    ## 477         date   4
    ## 478         dear   4
    ## 479         drew   4
    ## 480      driving   4
    ## 481        drunk   4
    ## 482       echoes   4
    ## 483    everytime   4
    ## 484        faded   4
    ## 485        faith   4
    ## 486       father   4
    ## 487       figure   4
    ## 488         fire   4
    ## 489          fit   4
    ## 490        flash   4
    ## 491     flawless   4
    ## 492    footsteps   4
    ## 493  forevermore   4
    ## 494      freedom   4
    ## 495        ghost   4
    ## 496       ghosts   4
    ## 497       giving   4
    ## 498         grab   4
    ## 499        grace   4
    ## 500         hang   4
    ##  [ reached 'max' / getOption("max.print") -- omitted 1457 rows ]

Unusurpsingly, “love” clocks in at \#2. But let’s not throw Taylor under
the bus here. Is it really true that all of her songs are about
relationships and heartbreaks? Did she really need to reinvent herself
in 2017 (and 2014, and 2012… you get the point) to escape that image?

    k <- 5 
    ldaOut.terms <- read.csv(file=paste("LDAGibbs_k=",k,"_TopicsToTerms.csv"))
    #topicProbabilities <- read.csv(file=paste0("LDAGibbs_k=",k,"_TopicProbabilities.csv"))
    #topic1ToTopic2 <- read.csv(file=paste0("LDAGibbs_k=",k,"_Topic1ToTopic2.csv"))

I used LDA to generate 5 topics, and each topic groups a set of words
together. The table below shpws that the topics don’t really have a
pattern (but Topic 3 clearly references “Shake it Off”). Topic 2 appears
to cover the most romantic ground.

    ldaOut.terms <- ldaOut.terms[,2:length(ldaOut.terms)]

    ldaOut.terms

    ##   Topic.2 Topic.3 Topic.4 Topic.5
    ## 1    love     ooh    girl    feel
    ## 2    time   shake    home   wanna
    ## 3    stay    call     eye    yeah
    ## 4    hand    wait   forev   light
    ## 5     bad   gonna    hold    game

The last bit of analysis I want to cover is determining whether topics
occur more frequently over time. Is Taylor releasing songs that fall
under a given topic more frequntly? Might the topics represent an album?

    ## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.

![](tswift-post_files/figure-markdown_strict/last-1.png)

“Year” and “Topic” do not appear to be correlated at all. The data show
that topics are spead out relatively evenly across years. Topic 5 seems
to be very prominent across songs in 2017. From the words assigned high
probability to Topic 5, including “wanna,” “feel,” and “game,” might it
be the case that these songs are representative of Taylor’s increasing
agency?

LDA revealed much of what I already knew, in that the topics of Taylor
Swift’s songs tend to be associated with love, feelings, dreaming (of
the romantic sort), etc. I do feel like now I know a little more about
the patterns hidden underneath some of her most popular songs, and maybe
a little more about what makes them so catchy.
