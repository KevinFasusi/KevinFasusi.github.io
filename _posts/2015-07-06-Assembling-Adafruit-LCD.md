---

layout: post
title: Assembling Adafruit LCD
excerpt: A little solder here, a little code there aaand &#46;&#46;&#46; I broke it!
---

{% include header.html %}

# A little solder here, a little code there aaand &#46;&#46;&#46; I broke it! #

I am just testing to see if this pagination shows my page content.

{% highlight ruby %}
def show
  @widget = Widget(params[:id])
  respond_to do |format|
    format.html # show.html.erb
    format.json { render json: @widget }
  end
end
{% endhighlight %}