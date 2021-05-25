<!-- ``` {r image, out.width = "200px", echo = FALSE} -->
<!-- knitr::include_graphics("Taylor_Swift_Cavendish.jpg") -->
<!-- ``` -->

# A Topic Modelling Analysis of Taylor Swift Song Lyrics 🎤

![](https://github.com/alliesaizan/teardrops-on-my-df/tree/main/assets/1989_World_Tour.jpg)

I was browsing Kaggle for an interesting dataset when I ran across a [library of Taylor Swift’s songs](https://www.kaggle.com/PromptCloudHQ/taylor-swift-song-lyrics-from-all-the-albums).

Usually when I find data on Kaggle, it’s been picked over and analyzed all sort of ways. But luckily for me, only one person had analyzed these data! I guess data scientists aren’t much into pop culture (or data not associated with million-dollar competitions).

I thought it might be fun to look for patterns in Taylor’s music. My running theory for the past decade is that Taylor’s songs all sound the same. Would the data bear that pattern out?

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

The data needs pre-processing, which includes removing stopwords and special characters and coercing every word to lowercase. Removing stopwords is a typical natural language processing task, but it gives me some anxiety because some of Taylor’s most prominent songs (such as “You Belong With Me”) contain stopwords that impact song meaning (“You”, “With”, “Me”).

In the original dataset, every row contains one song lyric, which is not ideal from a data tidyness point of view. The steps are hidden from view, but I end up with a reshaped dataset.

Before I began the LDA analysis, I did a quick exploratory analysis involving graphs that I can't show in this Markdown file. To view those graphs, take a look at the files in the [presentations](https://github.com/alliesaizan/teardrops-on-my-df/tree/main/presentations) folder.

Does Taylor Swift really sing about the same topics, or am I salty? To find out, I use a method called [Latent Dirichlet Allocation
(LDA)](https://eight2late.wordpress.com/2015/09/29/a-gentle-introduction-to-topic-modeling-using-r/). LDA is a generative probabilistic model commonly used to model topics in documents.

LDA assumes that each document is a mixture of topics, and each topic is a mixture of words. It can be used for many different types of analyses. The probabilistic model generated by LDA assigns probabilities to a word falling under a given topic, and to a topic being assigned to a given document.

I will treat each song as a “document.” Before we dive into the analysis, let’s look at the top 100 frequent words in her collection.

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

Unusurpsingly, “love” clocks in at \#2. But let’s not throw Taylor under the bus here. Is it really true that all of her songs are about
relationships and heartbreaks? Did she really need to reinvent herself in 2017 (and 2014, and 2012… you get the point) to escape that image?

    k <- 5 
    ldaOut.terms <- read.csv(file=paste("LDAGibbs_k=",k,"_TopicsToTerms.csv"))
    #topicProbabilities <- read.csv(file=paste0("LDAGibbs_k=",k,"_TopicProbabilities.csv"))
    #topic1ToTopic2 <- read.csv(file=paste0("LDAGibbs_k=",k,"_Topic1ToTopic2.csv"))

I used LDA to generate 5 topics, and each topic groups a set of words together. The table below shpws that the topics don’t really have a
pattern (but Topic 3 clearly references “Shake it Off”). Topic 2 appears to cover the most romantic ground.

    ##   Topic.2 Topic.3 Topic.4 Topic.5
    ## 1    love     ooh    girl    feel
    ## 2    time   shake    home   wanna
    ## 3    stay    call     eye    yeah
    ## 4    hand    wait   forev   light
    ## 5     bad   gonna    hold    game

The last bit of analysis I want to cover is determining whether topics occur more frequently over time. Is Taylor releasing songs that fall under a given topic more frequntly? Might the topics represent an album?

“Year” and “Topic” do not appear to be correlated at all. The data show that topics are spead out relatively evenly across years. Topic 5 seems to be very prominent across songs in 2017. From the words assigned high probability to Topic 5, including “wanna,” “feel,” and “game,” might it be the case that these songs are representative of Taylor’s increasing agency?

LDA revealed much of what I already knew, in that the topics of Taylor Swift’s songs tend to be associated with love, feelings, dreaming (of the romantic sort), etc. I do feel like now I know a little more about the patterns hidden underneath some of her most popular songs, and maybe a little more about what makes them so catchy. 
