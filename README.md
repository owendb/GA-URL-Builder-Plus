# GA-URL-Builder-Plus
Google Analytics URL tracked campaign link builder. Simplified URL parameter builder for use in orgs with multiple staff posting links,u create rules to link source and medium, ensure consistant spellings for campaign names and built in URL shortening with Bit.ly

## Intro
We built a [webpage tool](#) to replace <a href="https://support.google.com/analytics/answer/1033867?hl=en-GB" target="_blank">this</a> one by Google. so that users can paste in a link and get the url parameters they need to track their campaign in Google Analytics. It's been designed to let multiple users across an organisation add tracking in a consistent way that doesn't require them to have any knowledge of Google Analytics. Resulting in clean crisp campaign data having to worry about _email_ being both _utm&#95;medium_ and _utm&#95;source_ or worse still no tracking added at all. 

* [Demo](#)
* [Download it as a zip](https://github.com/owendb/GA-URL-Builder-Plus/archive/master.zip) (with setup instructions). 
* [Fork it on GitHub](https://github.com/owendb/GA-URL-Builder-Plus/fork).

##Instructions



Sign up to bit.ly. In the settings section copy the legacy api key and your user name. Open this file (index.html) in a text editor and find and replace the bit.ly account listed with your own account.

```
$(document).ready(function()
{
 function bit_url(url)
{
var url=url;
var username="";
var key="";
$.ajax({
url:"http://api.bit.ly/v3/shorten",
data:{longUrl:url,apiKey:key,login:username},
dataType:"jsonp",
success:function(v)
{
var bit_url=v.data.url;
$("#result").html('<a href="'+bit_url+'" target="_blank">'+bit_url+'</a>');
}
});
```


<li>
Add your source/medium combinations to the placement dropdown by finding this section of code in this file. The optgroup label don't affect any functionality but make it look a bit tidier and help reinforce the link between source and medium.<br><br>
<div class="well well-sm">

    &#x3C;select id=&#x22;utm_source&#x22; name=&#x22;utm_source&#x22; class=&#x22;input-xlarge&#x22; onchange=&#x22;setmedium(this.value); javascript: changeSubcat(this.options[this.selectedIndex].value);&#x22;&#x3E;<br><br>
            &#x3C;option&#x3E;-&#x3C;/option&#x3E;<br>
            &#x3C;optgroup label=&#x22;Social&#x22;&#x3E;<br>
            &#x3C;option value=&#x22;FacebookPost&#x22;&#x3E;Facebook Post&#x3C;/option&#x3E;<br>
            &#x3C;option value=&#x22;FacebookAd&#x22;&#x3E;Facebook Ad&#x3C;/option&#x3E;<br>
            &#x3C;option value=&#x22;twitter&#x22;&#x3E;Twitter&#x3C;/option&#x3E;<br>
            &#x3C;option value=&#x22;youtube&#x22;&#x3E;YouTube&#x3C;/option&#x3E;<br>
            &#x3C;option value=&#x22;pinterest&#x22;&#x3E;Pinterest&#x3C;/option&#x3E;<br>
            &#x3C;option value=&#x22;flickr&#x22;&#x3E;Flickr&#x3C;/option&#x3E;<br><br>
            &#x3C;optgroup label=&#x22;Email&#x22;&#x3E;<br>
            &#x3C;option value=&#x22;email_signature&#x22;&#x3E;Email signature&#x3C;/option&#x3E;<br>
            &#x3C;option value=&#x22;general_enews&#x22;&#x3E;Monthly Enews&#x3C;/option&#x3E;<br>
            &#x3C;option value=&#x22;single_enews&#x22;&#x3E;Single Enews&#x3C;/option&#x3E;<br>
            &#x3C;optgroup label=&#x22;Display&#x22;&#x3E;<br>
     &#x3C;option value=&#x22;Banner&#x22;&#x3E;Banner&#x3C;/option&#x3E;<br><br>
            &#x3C;optgroup label=&#x22;Outdoor&#x22;&#x3E;<br>
     &#x3C;option value=&#x22;Poster&#x22;&#x3E;Poster&#x3C;/option&#x3E;<br><br>
    &#x3C;/select&#x3E;<br>

<br>
</div>
</li>
<li>
Find this section of code and create any new rules you need. You'll need to add in any new sources you've added to the placement dropdown and add which medium it should select behind the scenes. In this case FacebookAd, twitter, youtube etc.. are sources and match the placement dropdown values, if they are selected 'social' will be added as the medium. "||" means OR. I.E. If you select FacebookAd or twitter or youtube as the source then the medium is social.<br><br>
<div class="well well-sm">
&#x3C;script type=&#x22;text/javascript&#x22;&#x3E;<br>
    &#x3C;!--<br>
<br>
    function setmedium(source) {<br>
<br>
        mysource = source;<br>
<br>
        if (mysource == &#x22;<strong>FacebookAd</strong>&#x22; || mysource == &#x22;<strong>twitter</strong>&#x22; || mysource == &#x22;<strong>youtube</strong>&#x22; || mysource == &#x22;<strong>flickr</strong>&#x22; || mysource == &#x22;<strong>pinterest</strong>&#x22; || mysource == &#x22;<strong>FacebookPost</strong>&#x22;) {<br>
            document.ctm.utm_medium.value = &#x22;<strong>social</strong>&#x22;;}<br><br>
         else if (mysource == &#x22;email_signature&#x22; || mysource == &#x22;general_enews&#x22; || mysource == &#x22;single_enews&#x22; || mysource == &#x22;email_signature&#x22;) {<br>
<br>
            document.ctm.utm_medium.value = &#x22;email&#x22;;}<br>
<br>
         else if (mysource == &#x22;newsadvert&#x22; || mysource == &#x22;newsarticle&#x22; || mysource == &#x22;pressrelease&#x22;) {<br>
<br>
            document.ctm.utm_medium.value = &#x22;press&#x22;;}<br>
<br>
         else if (mysource == &#x22;poster&#x22;) {<br>
<br>
            document.ctm.utm_medium.value = &#x22;outdoor&#x22;;}<br>
<br>
    }<br>
&#x3C;/script&#x3E;<br>
</div>

</li>
<li>
Add your campaigns to the campaign dropdown. Update as you get new campaigns<br><br>
<div class="well well-sm">

    &#x3C;select id=&#x22;utm_campaign&#x22; name=&#x22;utm_campaign&#x22; class=&#x22;input-xlarge&#x22;&#x3E;<br>
              &#x3C;option&#x3E;-&#x3C;/option&#x3E;<br>
              &#x3C;option&#x3E;London Marathon&#x3C;/option&#x3E;<br>
              &#x3C;option&#x3E;Research News&#x3C;/option&#x3E;<br>
              &#x3C;option&#x3E;Ice Bucket Challenge&#x3C;/option&#x3E;<br>
              &#x3C;option&#x3E;Our Great Campaign&#x3C;/option&#x3E;<br>
    &#x3C;/select&#x3E;
</div>
</li>
<li>
Host the whole folder on a network drive or on an internal server (the bit.ly api key is stored in the open so you don't want this somewhere publically accessible, the trade off in security makes the tool a more portable and easily editable. We put it on a shared network drive)
</li>  
</ul>
</div>
Yes please do, fork away. It could do with a couple of things tweaking

- Move from the depreciated bit.ly api we're currently using.
- Suss out a more elegant way to add campaigns rather than editting the HTML. Google Sheets? Separate text file?

## Credits

Owen & Jim @ Leukaemia & Lymphoma Research

Tom @ The Royal Marsden Charity

