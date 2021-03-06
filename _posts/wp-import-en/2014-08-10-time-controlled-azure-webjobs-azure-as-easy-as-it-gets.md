---
layout: post
title: "Time-controlled Azure WebJobs – Azure as easy as it get‘s"
date: 2014-08-10 12:36
author: CI Team
comments: true
categories: [Uncategorized]
tags: []
language: en
---
{% include JB/setup %}
<a href="{{BASE_PATH}}/assets/wp-images-en/image2025-570x194.png"><img title="image2025-570x194" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="image2025-570x194" src="{{BASE_PATH}}/assets/wp-images-en/image2025-570x194_thumb.png" width="583" height="211"></a>

<p>While still in development the <a href="http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx">Azure WebJob SDK</a> offers some cool features for procession and supply of information. A good example is the sample that observes the Azure Queue and processes an item as soon as it spots one. </p> 
<p><b>Scenario: time-controlled activities – without queue and so on </b></p>
<p>My scenario was quite simple. I was searching for a way to open a method time-controlled and store the data in the blob storage – without a cloud. </p>

<p><b>The code:</b></p> 

<div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:ff9df87c-2082-40b7-b126-57c03df5d0d8" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"><pre style=" width: 602px; height: 292px;background-color:White;overflow: auto;"><div><!--

Code highlighting produced by Actipro CodeHighlighter (freeware)
http://www.CodeHighlighter.com/

--><span style="color: #800080;">1</span><span style="color: #000000;">: </span><span style="color: #0000FF;">class</span><span style="color: #000000;"> Program
   </span><span style="color: #800080;">2</span><span style="color: #000000;">:     {
   </span><span style="color: #800080;">3</span><span style="color: #000000;">:         </span><span style="color: #0000FF;">static</span><span style="color: #000000;"> </span><span style="color: #0000FF;">void</span><span style="color: #000000;"> Main(</span><span style="color: #0000FF;">string</span><span style="color: #000000;">[] args)
   </span><span style="color: #800080;">4</span><span style="color: #000000;">:         {
   </span><span style="color: #800080;">5</span><span style="color: #000000;">:             JobHost host </span><span style="color: #000000;">=</span><span style="color: #000000;"> </span><span style="color: #0000FF;">new</span><span style="color: #000000;"> JobHost();
   </span><span style="color: #800080;">6</span><span style="color: #000000;">:             host.Call(</span><span style="color: #0000FF;">typeof</span><span style="color: #000000;">(Program).GetMethod(</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">WriteFile</span><span style="color: #800000;">&quot;</span><span style="color: #000000;">));
   </span><span style="color: #800080;">7</span><span style="color: #000000;">:         }
   </span><span style="color: #800080;">8</span><span style="color: #000000;">:  
   </span><span style="color: #800080;">9</span><span style="color: #000000;">:         [NoAutomaticTrigger]
  </span><span style="color: #800080;">10</span><span style="color: #000000;">:         </span><span style="color: #0000FF;">public</span><span style="color: #000000;"> </span><span style="color: #0000FF;">static</span><span style="color: #000000;"> </span><span style="color: #0000FF;">void</span><span style="color: #000000;"> WriteFile([Blob(</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">container/foobar.txt</span><span style="color: #800000;">&quot;</span><span style="color: #000000;">)]TextWriter writer)
  </span><span style="color: #800080;">11</span><span style="color: #000000;">:         {
  </span><span style="color: #800080;">12</span><span style="color: #000000;">:             writer.WriteLine(</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">Hello World...</span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> </span><span style="color: #000000;">+</span><span style="color: #000000;"> DateTime.UtcNow.ToShortDateString() </span><span style="color: #000000;">+</span><span style="color: #000000;"> </span><span style="color: #800000;">&quot;</span><span style="color: #800000;"> - </span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> </span><span style="color: #000000;">+</span><span style="color: #000000;"> DateTime.UtcNow.ToShortTimeString());
  </span><span style="color: #800080;">13</span><span style="color: #000000;">:         }
  </span><span style="color: #800080;">14</span><span style="color: #000000;">:     }</span></div></pre><!-- Code inserted with Steve Dunn's Windows Live Writer Code Formatter Plugin.  http://dunnhq.com --></div>
  
<p>If you have a look on the most popular examples you might recognize that the method “RunAndBlock” won’t be started. That is also not necessary since the program will be “woken” by the scheduler to open the “write” method. </p> 

<p>To get the code to work you have to deposit the following configurations. Afterwards you are able to zip the whole console application and upload it into the Azure portal and configure the scheduler. </p> 

<p>Configurations:</p>

<div id="scid:9D7513F9-C04C-4721-824A-2B34F0212519:6a342399-8728-4682-b099-292e5b3f8e7e" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"><pre style=" width: 704px; height: 312px;background-color:White;overflow: auto;"><div><!--

Code highlighting produced by Actipro CodeHighlighter (freeware)
http://www.CodeHighlighter.com/

--><span style="color: #000000;">  </span><span style="color: #800080;">1</span><span style="color: #000000;">: </span><span style="color: #000000;">&lt;?</span><span style="color: #000000;">xml version</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">1.0</span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> encoding</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">utf-8</span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> </span><span style="color: #000000;">?&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">2</span><span style="color: #000000;">: </span><span style="color: #000000;">&lt;</span><span style="color: #000000;">configuration</span><span style="color: #000000;">&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">3</span><span style="color: #000000;">:   </span><span style="color: #000000;">&lt;</span><span style="color: #000000;">connectionStrings</span><span style="color: #000000;">&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">4</span><span style="color: #000000;">:     </span><span style="color: #000000;">&lt;</span><span style="color: #000000;">add name</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">AzureJobsDashboard</span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> connectionString</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">DefaultEndpointsProtocol=https;AccountName=...;AccountKey=...</span><span style="color: #800000;">&quot;</span><span style="color: #000000;">/&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">5</span><span style="color: #000000;">:     </span><span style="color: #000000;">&lt;</span><span style="color: #000000;">add name</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">AzureJobsStorage</span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> connectionString</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">DefaultEndpointsProtocol=https;AccountName=...;AccountKey=...</span><span style="color: #800000;">&quot;</span><span style="color: #000000;">/&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">6</span><span style="color: #000000;">:   </span><span style="color: #000000;">&lt;/</span><span style="color: #000000;">connectionStrings</span><span style="color: #000000;">&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">7</span><span style="color: #000000;">:   </span><span style="color: #000000;">&lt;</span><span style="color: #000000;">startup</span><span style="color: #000000;">&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">8</span><span style="color: #000000;">:     </span><span style="color: #000000;">&lt;</span><span style="color: #000000;">supportedRuntime version</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">v4.0</span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> sku</span><span style="color: #000000;">=</span><span style="color: #800000;">&quot;</span><span style="color: #800000;">.NETFramework,Version=v4.5</span><span style="color: #800000;">&quot;</span><span style="color: #000000;"> </span><span style="color: #000000;">/&gt;</span><span style="color: #000000;">
   </span><span style="color: #800080;">9</span><span style="color: #000000;">:   </span><span style="color: #000000;">&lt;/</span><span style="color: #000000;">startup</span><span style="color: #000000;">&gt;</span><span style="color: #000000;">
  </span><span style="color: #800080;">10</span><span style="color: #000000;">: </span><span style="color: #000000;">&lt;/</span><span style="color: #000000;">configuration</span><span style="color: #000000;">&gt;</span></div></pre><!-- Code inserted with Steve Dunn's Windows Live Writer Code Formatter Plugin.  http://dunnhq.com --></div>
  
<p><b>After the upload:</b></p> 

<p>The “WebJobs” are living inside the Azure Website. If you require a constantly running WebJob you have to make sure that the Azure website runs with “AlwaysOn=True”!</p> 

<p><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; padding-right: 0px" border="0" alt="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb1160.png" width="570" height="119"></p> 

<p>There is also a small administration portal for the WebJobs:</p> 

<p><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; padding-right: 0px" border="0" alt="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb1161.png" width="570" height="354"></p> 

<p>On the storage side a container will be created – including the container I’m using n the console.</p> 

<p><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; padding-right: 0px" border="0" alt="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb1162.png" width="570" height="152"></p>

<p>And of course the data is also available:</p>

<p><img title="image" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; padding-right: 0px" border="0" alt="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb1163.png" width="570" height="166"></p> 

<p><b>Result</b></p> 

<p>The first steps seem easy but at the same time quite clever. I like that so far. </p> 
<p>The (not to complicated) code is from a developer of the WebJob team on <a href="http://stackoverflow.com/questions/24486765/scheduled-azure-webjob-but-noautomatictrigger-method-not-invoked">Stackoverflow</a>. Of course it is also available on <a href="https://github.com/Code-Inside/Samples/tree/master/2014/WebJobsPlayground">GitHub</a>.</p> 
