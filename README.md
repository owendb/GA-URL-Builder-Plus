# GA-URL-Builder-Plus
Google Analytics URL tracked campaign link builder. Simplified URL parameter builder for use in orgs with multiple staff posting links, create rules to link source and medium, ensure consistant spellings for campaign names and built in URL shortening with Bit.ly

## Intro
We built a [webpage tool](#) to replace <a href="https://support.google.com/analytics/answer/1033867?hl=en-GB" target="_blank">this</a> one by Google. so that users can paste in a link and get the url parameters they need to track their campaign in Google Analytics. It's been designed to let multiple users across an organisation add tracking in a consistent way that doesn't require them to have any knowledge of Google Analytics. Resulting in clean crisp campaign data having to worry about _email_ being both _utm&#95;medium_ and _utm&#95;source_ or worse still no tracking added at all. 

* [Demo](#)
* [Download it as a zip](https://github.com/owendb/GA-URL-Builder-Plus/archive/master.zip) (with setup instructions). 
* [Fork it on GitHub](https://github.com/owendb/GA-URL-Builder-Plus/fork).

##Instructions



Sign up to bit.ly. In the settings section copy the legacy api key and your user name. Open this file (index.html) in a text editor and find and replace the bit.ly username and key listed with your own account.

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



Add your source/medium combinations to the placement dropdown by finding this section of code in this file. The optgroup label don't affect any functionality but make it look a bit tidier and help reinforce the link between source and medium.
```
    <select id="utm_source" name="utm_source" class="input-xlarge" onchange="setmedium(this.value); javascript: changeSubcat(this.options[this.selectedIndex].value);">
            <option>-</option>
            <optgroup label="Social">
            <option value="FacebookPost">Facebook Post</option>
            <option value="FacebookAd">Facebook Ad</option>
            <option value="twitter">Twitter</option>
            <option value="youtube">YouTube</option>
            <option value="pinterest">Pinterest</option>
            <option value="flickr">Flickr</option>
            <optgroup label="Email">
            <option value="email_signature">Email signature</option>
            <option value="general_enews">Monthly Enews</option>
            <option value="single_enews">Single Enews</option>
            <optgroup label="Display">
     <option value="Banner">Banner</option>
            <optgroup label="Outdoor">
     <option value="Poster">Poster</option>
    </select>
```

Find this section of code and create any new rules you need. You'll need to add in any new sources you've added to the placement dropdown and add which medium it should select behind the scenes. In this case FacebookAd, twitter, youtube etc.. are sources and match the placement dropdown values, if they are selected 'social' will be added as the medium. "||" means OR. I.E. If you select FacebookAd or twitter or youtube as the source then the medium is social.

```
<script type="text/javascript">
    function setmedium(source) {
        mysource = source;

        if (mysource == "FacebookAd" || mysource == "twitter" || mysource == "youtube" || mysource == "flickr" || mysource == "pinterest" || mysource == "FacebookPost") {
            document.ctm.utm_medium.value = "social";}
         else if (mysource == "email_signature" || mysource == "general_enews" || mysource == "single_enews" || mysource == "email_signature") {
            document.ctm.utm_medium.value = "email";}

         else if (mysource == "newsadvert" || mysource == "newsarticle" || mysource == "pressrelease") {

            document.ctm.utm_medium.value = "press";}

         else if (mysource == "poster") {

            document.ctm.utm_medium.value = "outdoor";}

    }
</script>
```


Add your campaigns to the campaign dropdown. Update as you get new campaigns
```

    <select id="utm_campaign" name="utm_campaign" class="input-xlarge">
              <option>-</option>
              <option>London Marathon</option>
              <option>Research News</option>
              <option>Ice Bucket Challenge</option>
              <option>Our Great Campaign</option>
    </select>
```

Host the whole folder on a network drive or on an internal server (the bit.ly api key is stored in the open so you don't want this somewhere publically accessible, the trade off in security makes the tool a more portable and easily editable. We put it on a shared network drive)

### Can I help?
Yes please do, fork away. It could do with a couple of things tweaking

- Move from the depreciated bit.ly api we're currently using.
- Suss out a more elegant way to add campaigns rather than editting the HTML. Google Sheets? Separate text file?

## Credits

Owen & Jim @ Leukaemia & Lymphoma Research

Tom @ The Royal Marsden Charity

