---

layout: post
title: Naive Bayes Document Classifier Part 2
excerpt: Building the beast
---

{% include header.html %}

# Naive Bayes Document Classifier Part 2-The good, the bad, the VBA #


The objective is to automate the whole process; I wanted to drop the files in the folder, and that is it. Using Microsoft Scripting Runtime object library, I used FileSystemObject and textstream instead of the input-output method. The 'classifier training' class has a method for extracting and tokenizing the text a sentence at a time from each file in the directory. The '.bas' files are in my repo here.

The first code snippet below shows the iteration through the directory tokenizing the text, removing punctuation and creating the 'corpuArr' which holds all the tokenized text from every file placed in the folder. At this point the array will hold duplicates, this is intentional. The duplicates will be removed later when every instance of the same word is tallied.

## Removing Punctuation and Tokenizing. ##

{% highlight BASIC linenos %}
Do While filePath <> ""
        Debug.Print filePath
        If filePath <> "ModelData.txt" Then
            Do While fsoReadStream.AtEndOfStream <> True
                sentenceText = fsoReadStream.ReadLine
                sentenceText = RemoveArtifacts(sentenceText)
                If sentenceText <> "" Then
                    sentenceArr() = TokenizeSentence(sentenceText)                
                    ReDim Preserve corpusArr(j + UBound(sentenceArr()))
                        For i = 0 To UBound(sentenceArr())
                            corpusArr(i + j) = sentenceArr(i)
                        Next
                    j = j + UBound(sentenceArr())
                Else: End If       
            Loop
        Else
        End If
        filePath = Dir()
    
        fsoReadStream.Close
        Set fsoSourceFile = fsoSource.GetFile(folderPath & pathSep & filePath)
        Set fsoReadStream = fsoSourceFile.OpenAsTextStream(ForReading, TristateUseDefault)
Loop
{% endhighlight %}


Below is the method for removing punctuation, it's not pretty but it gets the job done.


{% highlight BASIC linenos %}
Private Function RemoveArtifacts(sentence As String) As String

    RemoveArtifacts = Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(Replace(LCase(sentence), ". ", " "), ", ", " "), "; ", " "), ": ", " ") _
                                , "' ", " "), "! ", " "), "# ", " "), "$", " "), "%", " "), "&", " "), "(", " "), ")", " "), " - ", " "), "_", " "), "--", " "), "+", " ") _
                                , "=", " "), "/", " "), "\", " "), "{", " "), "}", " "), "[", " "), "]", " "), """ ", " "), "?", " "), "*", " "), " """, " "), """", " ")
                                
End Function
{% endhighlight %}

Below is the method for tokenizing senetence.


{% highlight BASIC linenos %}
Private Function TokenizeSentence(sentence As String) As Variant
Dim i, counter As Integer
Dim bagOfCharArr() As String
Dim bagOfWordsArr() As String

counter = 0
    bagOfCharArr() = Split(sentence)
    ReDim bagOfWordsArr(UBound(bagOfCharArr()))
        For i = 0 To UBound(bagOfCharArr())
            If Not Replace(Trim(bagOfCharArr(i)), " ", "") = "" Then
                bagOfWordsArr(counter) = bagOfCharArr(i)
                counter = counter + 1
            Else: End If
        Next
    If counter <> 0 Then
    ReDim Preserve bagOfWordsArr(counter - 1)
    Else: End If
        
wordArr() = bagOfWordsArr()
TokenizeSentence = bagOfWordsArr()
End Function
{% endhighlight %}

## Stop Words ##

At this stage it is normal to remove 'stop words', these are words with little lexical content (Foreman, 2014:91). However, as advised in the data smart book, removing all words with four characters or less proved a useful quick and dirty method. I had planned on incorporating a list of stop words and I may yet still. 

## Tokens and Probabilities

After tokenizing the sentences and removing the punctuation, the next step is to count the tokens and calculate the conditional probabilities. The cleaned tokens (stop words and punctuation removed) are tallied, and the frequency of each word is stored in a 2-dimentional array (tallyCorpusArr). 

{% highlight BASIC linenos %}

wordFromArr = cleanCorpusArr(i, 0)
If wordFromArr <> "" Then
    matchCondition = chkArray(wordFromArr, tallyCorpusArr)
        If matchCondition = False Then
            tallyCorpusArr(p, 0) = cleanCorpusArr(i, 0)
            j = 0
            tallyCount = 0
            Do Until j = UBound(cleanCorpusArr())
                If tallyCorpusArr(p, 0) = cleanCorpusArr(j, 0) Then
                    tallyCount = tallyCount + 1
                    tallyCorpusArr(p, 1) = tallyCount
                Else: End If
                 j = j + 1
            Loop
              p = p + 1
        Else: End If
Else: End If

Next
{% endhighlight %}

The cumulative total of all the tokens is then calculated and used to calculate the probability of each token in the corpus. The normal logarithmic probability is calculated using the LN function. The actual tally, additive smoothed tally, probability and LN probability are then added to an array before writing to a text file. An example of the files content is below.

<table>
    <thead>
        <tr>
            <th>Token</th>
            <th>Tally Count</th>
            <th>Additive Smoothed Tally</th>
            <th>Probability</th>
            <th>Logarithmic Probability</th>
        </tr>
    </thead>
    <tr>
        <td>information</td>
        <td>207</td>
        <td>208</td>
        <td>0.0041823337</td>
        <td>2.3159703876</td>
    </tr>
    <tr>
        <td>management</td>
        <td>355</td>
        <td>356</td>
        <td>0.0071582249</td>
        <td>2.5502283777</td>
    </tr>
</table>
 

