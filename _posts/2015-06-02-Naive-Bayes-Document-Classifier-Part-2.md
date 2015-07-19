---

layout: post
title: Naive Bayes Document Classifier Part 2
excerpt: Building the beast
---

{% include header.html %}

# Building The Classifier #


Since the objective is to automate the whole process, I wanted to drop the files in the folder and be done with it. Using Microsoft Scripting runtime object library, I used FileSystemObject and textstream instead of the input output method. The 'classifier training' class has a method for extracting and tokenizing the text a sentence at a time from each file in the directory. The '.bas' files can be found in my repo here.

The first code snippet below shows the interation through the directory removing punctuation, tokenizing the text and creating the 'corpuArr' which holds all the takenized text from every file placed in the folder. At this point the array will hold duplicates, this is intentional. The duplicates will be removed later, when the number of occurances for each word is tallied.

Extracting, removing punctuation and tokenizing.
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

At this stage it is normal to remove 'stop words', these are words with little lexical content (Foreman, 2014:91). However removing all words with 4 charaters or less proved a useful quick and dirty method. I had planned on incorporating a list of stop words and I may yet still. 

After tokenizing the sentences and removing the punctuation, the next step is to count the tokens and calculate the conditional probabilities. 

{% highlight BASIC linenos}

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
{% endhiglight %}