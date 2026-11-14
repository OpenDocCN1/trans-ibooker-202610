
Use the `Invoke-WebRequest` cmdlet to download a web page, and then access the `Content` property (or cast the result to a `[string]`):

```powershell
PS > $source = "http://www.bing.com/search?q=sqrt(2)"
PS > $result = [string] (Invoke-WebRequest $source)
```

## Discussion

When writing automation in a web-connected world, we aren't always fortunate enough to have access to a web service that returns richly structured data. Because of this, retrieving data from services on the internet often comes by means of *screen scraping*: downloading the HTML of the web page and then carefully separating out the content you want from the vast majority of the content that you do not.

### Note

If extracting structured data from a web page is your primary goal, the `Invoke-WebRequest` cmdlet offers options much more powerful than basic screen scraping. For more information, see [Recipe 12.5](#ch12.xhtml#parse_analyze_web_page).

The technique of screen scraping has been around much longer than the internet! As long as computer systems have generated output designed primarily for humans, screen scraping tools have risen to make this output available to other computer programs.

Unfortunately, screen scraping is an error-prone way to extract content. And that's no exaggeration! As proof, [Example 12-6](#ch12.xhtml#get-answerps1) (shown later in this recipe) broke four or five times while the first edition of this book was being written, and then again after it was published. Then it broke several times during the second edition, and again after it was published. Then, it broke after the third edition was published. Although it was fixed for the fourth edition, it's likely broken as you read this. Such are the perils of screen scraping.

If the web page authors change the underlying HTML, your code will usually stop working correctly. If the site's HTML is written as valid XHTML, you may be able to use PowerShell's built-in XML support to more easily parse the content.

For more information about PowerShell's built-in XML support, see [Recipe 10.1](#ch10.xhtml#access_information_in_an_xml_file).

Despite its fragility, pure screen scraping is often the only alternative. Since screen scraping is just text manipulation, you have the same options you do with other text reports. For some fairly structured web pages, you can get away with a single regular expression replacement (plus cleanup), as shown in [Example 12-5](#ch12.xhtml#search-bingps1).

### Example 12-5. Search-Bing.ps1

```powershell
##############################################################################
##
## Search-Bing
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Search Bing for a given term

.EXAMPLE

PS > Search-Bing PowerShell
Searches Bing for the term "PowerShell"

#>

param(
    ## The term to search for
    $Pattern = "PowerShell"
)

Set-StrictMode -Version 3

## Create the URL that contains the Bing search results
Add-Type -Assembly System.Web
$queryUrl = 'http://www.bing.com/search?q={0}'
$queryUrl = $queryUrl -f ([System.Web.HttpUtility]::UrlEncode($pattern))

## Download the web page
$results = [string] (Invoke-WebRequest $queryUrl)

## Extract the text of the results, which are contained in
## segments that look like "<div class="sb_tlst">...</div>"
$matches = $results |
    Select-String -Pattern '(?s)<div[^>]*sb_tlst[^>]*>.*?</div>' -AllMatches

foreach($match in $matches.Matches)
{
    ## Extract the URL, keeping only the text inside the quotes
    ## of the HREF
    $url = $match.Value -replace '.*href="(.*?)".*','$1'
    $url = [System.Web.HttpUtility]::UrlDecode($url)

    ## Extract the page name,  replace anything in angle
    ## brackets with an empty string.
    $item = $match.Value -replace '<[^>]*>', ''

    ## Output the item
    [PSCustomObject] @{ Item = $item; Url = $url }
}
```

Text parsing on less structured web pages, while possible to accomplish with complicated regular expressions, can often be made much simpler through more straightforward text manipulation. [Example 12-6](#ch12.xhtml#get-answerps1) uses this second approach to fetch "Instant Answers" from Bing.

### Example 12-6. Get-Answer.ps1

```powershell
##############################################################################
##
## Get-Answer
##
## From PowerShell Cookbook (O'Reilly)
## by Lee Holmes (http://www.leeholmes.com/guide)
##
##############################################################################

<#

.SYNOPSIS

Uses Bing Answers to answer your question

.EXAMPLE

PS > Get-Answer "sqrt(2)"
sqrt(2) = 1.41421356

.EXAMPLE

PS > Get-Answer msft stock
Microsoft Corp (US:MSFT) NASDAQ
29.66  -0.35 (-1.17%)
After Hours: 30.02 +0.36 (1.21%)
Open: 30.09    Day's Range: 29.59 - 30.20
Volume: 55.60 M    52 Week Range: 17.27 - 31.50
P/E Ratio: 16.30    Market Cap: 260.13 B

.EXAMPLE

PS > Get-Answer "What is the time in Seattle, WA?"
Current time in Seattle, WA
01:12:41 PM
08/18/2012 ? Pacific Daylight Time

#>

Set-StrictMode -Version 3

$question = $args -join " "

function Main
{
    ## Load the System.Web.HttpUtility DLL, to let us URLEncode
    Add-Type -Assembly System.Web

    ## Get the web page into a single string with newlines between
    ## the lines.
    $encoded = [System.Web.HttpUtility]::UrlEncode($question)
    $url = "http://www.bing.com/search?q=$encoded"
    $text = [String] (Invoke-WebRequest $url)

    ## Find the start of the answers section
    $startIndex = $text.IndexOf('<li class="b_ans')

    ## The end may be defined by one of the following strings, depending on the
    ## answer type. Pick the first.
    $endStrings = "Was this helpful","finf_mod",'<li class="b_algo',
        "People also search for","Feedback"
    $answerIndexes = $endStrings | Foreach-Object { $text.IndexOf($_, $startIndex) } |
        Where-Object { $_ -ge 0 }
    $endIndex = ($answerIndexes | Sort-Object)[0]

    ## If we found a result, then filter the result
    if(($startIndex -ge 0) -and ($endIndex -ge 0))
    {
        ## Pull out the text between the start and end portions
        $partialText = $text.Substring($startIndex, $endIndex - $startIndex)

        ## Very fragile screen scraping here. Replace a bunch of
        ## tags that get placed on new lines with the newline
        ## character, and a few others with spaces.
        $partialText = $partialText -replace '<div[^>]*>',"`n"
        $partialText = $partialText -replace '<tr[^>]*>',"`n"
        $partialText = $partialText -replace '<li[^>]*>',"`n"
        $partialText = $partialText -replace '<br[^>]*>',"`n"
        $partialText = $partialText -replace '<p [^>]*>',"`n"
        $partialText = $partialText -replace '<span[^>]*>'," "
        $partialText = $partialText -replace '<td[^>]*>',"    "

        $partialText = CleanHtml $partialText

        ## Now split the results on newlines, trim each line, and then
        ## join them back.
        $partialText = $partialText -split "`n" |
            Foreach-Object { $_.Trim() } | Where-Object { $_ }
        $partialText = $partialText -join "`n"

        [System.Web.HttpUtility]::HtmlDecode($partialText.Trim())
    }
    else
    {
        "No answer found."
    }
}

## Clean HTML from a text chunk
function CleanHtml ($htmlInput)
{
    $tempString = [Regex]::Replace($htmlInput, "(?s)<[^>]*>", "")
    $tempString.Replace("&nbsp&nbsp", "")
}

Main
```

When using the `Invoke-WebRequest` cmdlet, you might notice some web applications acting oddly or returning an error that you're using an unsupported browser.
