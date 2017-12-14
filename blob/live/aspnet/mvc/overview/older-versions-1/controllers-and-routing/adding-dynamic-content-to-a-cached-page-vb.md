---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: "将动态内容添加到缓存的页面 (VB) |Microsoft 文档"
author: microsoft
description: "了解如何混合同一页中的动态和缓存内容。 缓存后替换使您能够显示动态内容，如横幅播发 o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: f07f4ecec36e71679dbc471b65f26d260349a07e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a><span data-ttu-id="81672-104">将动态内容添加到缓存的页面 (VB)</span><span class="sxs-lookup"><span data-stu-id="81672-104">Adding Dynamic Content to a Cached Page (VB)</span></span>
====================
<span data-ttu-id="81672-105">通过[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="81672-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="81672-106">了解如何混合同一页中的动态和缓存内容。</span><span class="sxs-lookup"><span data-stu-id="81672-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="81672-107">缓存后替换，你可以显示动态内容，如横幅播发或用于缓存已输出的页中的新闻的项。</span><span class="sxs-lookup"><span data-stu-id="81672-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="81672-108">通过利用的输出缓存，你可以显著地提高 ASP.NET MVC 应用程序的性能。</span><span class="sxs-lookup"><span data-stu-id="81672-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="81672-109">而不是重新生成一个页面请求该页的每个时间，可以一次生成页，并在多个用户的内存中缓存中。</span><span class="sxs-lookup"><span data-stu-id="81672-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="81672-110">但没有问题。</span><span class="sxs-lookup"><span data-stu-id="81672-110">But there is a problem.</span></span> <span data-ttu-id="81672-111">如果你需要在页中显示动态内容？</span><span class="sxs-lookup"><span data-stu-id="81672-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="81672-112">例如，假设你想要在页中显示横幅播发。</span><span class="sxs-lookup"><span data-stu-id="81672-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="81672-113">你不希望横幅播发将缓存，以便每个用户将看到非常相同的播发。</span><span class="sxs-lookup"><span data-stu-id="81672-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="81672-114">你不会进行任何 money 这样 ！</span><span class="sxs-lookup"><span data-stu-id="81672-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="81672-115">幸运的是，这里有一个简单的解决方案。</span><span class="sxs-lookup"><span data-stu-id="81672-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="81672-116">你可以利用的 ASP.NET 框架调用一项功能*缓存后替换*。</span><span class="sxs-lookup"><span data-stu-id="81672-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="81672-117">缓存后替换使你可以用在页中已缓存在内存中的动态内容。</span><span class="sxs-lookup"><span data-stu-id="81672-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="81672-118">通常情况下，当你的输出缓存使用的页&lt;OutputCache&gt;属性，页面缓存服务器和客户端 （web 浏览器） 上。</span><span class="sxs-lookup"><span data-stu-id="81672-118">Normally, when you output cache a page by using the &lt;OutputCache&gt; attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="81672-119">当使用缓存后替换时，页面将缓存仅在服务器上。</span><span class="sxs-lookup"><span data-stu-id="81672-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="81672-120">使用缓存后替换</span><span class="sxs-lookup"><span data-stu-id="81672-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="81672-121">使用缓存后替换需要两个步骤。</span><span class="sxs-lookup"><span data-stu-id="81672-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="81672-122">首先，你需要定义返回一个字符串，表示你想要在缓存的页中显示的动态内容的方法。</span><span class="sxs-lookup"><span data-stu-id="81672-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="81672-123">接下来，你调用 HttpResponse.WriteSubstitution() 方法将动态内容插入页。</span><span class="sxs-lookup"><span data-stu-id="81672-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="81672-124">例如，假设你想要随机中缓存的页面中显示不同新闻项。</span><span class="sxs-lookup"><span data-stu-id="81672-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="81672-125">列表 1 中的类公开一个名为 RenderNews()，随机从列表中的三个新闻项返回一个新闻项的单个方法。</span><span class="sxs-lookup"><span data-stu-id="81672-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="81672-126">**列表 1 – Models\News.vb**</span><span class="sxs-lookup"><span data-stu-id="81672-126">**Listing 1 – Models\News.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

<span data-ttu-id="81672-127">若要充分利用缓存后替换，你可以调用 HttpResponse.WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="81672-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="81672-128">WriteSubstitution() 方法设置代码，来替换动态内容的缓存的页面区域。</span><span class="sxs-lookup"><span data-stu-id="81672-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="81672-129">WriteSubstitution() 方法用于列出 2 中的视图中显示的随机新闻项。</span><span class="sxs-lookup"><span data-stu-id="81672-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="81672-130">**列出 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="81672-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

<span data-ttu-id="81672-131">RenderNews 方法传递给 WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="81672-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="81672-132">请注意，不调用 RenderNews 方法。</span><span class="sxs-lookup"><span data-stu-id="81672-132">Notice that the RenderNews method is not called.</span></span> <span data-ttu-id="81672-133">而是对方法的引用传递给 WriteSubstitution() AddressOf 运算符的帮助。</span><span class="sxs-lookup"><span data-stu-id="81672-133">Instead a reference to the method is passed to WriteSubstitution() with the help of the AddressOf operator.</span></span>

<span data-ttu-id="81672-134">索引视图将被缓存。</span><span class="sxs-lookup"><span data-stu-id="81672-134">The Index view is cached.</span></span> <span data-ttu-id="81672-135">该视图会返回列出 3 中的控制器。</span><span class="sxs-lookup"><span data-stu-id="81672-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="81672-136">请注意，index （） 操作用修饰&lt;OutputCache&gt;导致要为 60 秒缓存的索引视图的属性。</span><span class="sxs-lookup"><span data-stu-id="81672-136">Notice that the Index() action is decorated with an &lt;OutputCache&gt; attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="81672-137">**列出 3 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="81672-137">**Listing 3 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

<span data-ttu-id="81672-138">即使缓存的索引视图，在请求索引页时，会显示不同的随机新闻项。</span><span class="sxs-lookup"><span data-stu-id="81672-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="81672-139">在请求索引页时，显示的页面的时间将不会更改 （请参见图 1） 的 60 秒。</span><span class="sxs-lookup"><span data-stu-id="81672-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="81672-140">时间不会更改的事实证明会缓存该页面。</span><span class="sxs-lookup"><span data-stu-id="81672-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="81672-141">但是，内容所插入 WriteSubstitution() 方法-随机新闻项目-更改与每个请求。</span><span class="sxs-lookup"><span data-stu-id="81672-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="81672-142">**图 1 – 将注入动态新闻项中缓存的页面**</span><span class="sxs-lookup"><span data-stu-id="81672-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="81672-144">生成的帮助程序方法中使用缓存后替换</span><span class="sxs-lookup"><span data-stu-id="81672-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="81672-145">更简单的方法以利用缓存后替换是封装对自定义帮助程序方法内 WriteSubstitution() 方法的调用。</span><span class="sxs-lookup"><span data-stu-id="81672-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="81672-146">通过列出 4 中的帮助器方法，此方法进行了阐释。</span><span class="sxs-lookup"><span data-stu-id="81672-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="81672-147">**列出 4 – Helpers\AdHelper.vb**</span><span class="sxs-lookup"><span data-stu-id="81672-147">**Listing 4 – Helpers\AdHelper.vb**</span></span>

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

<span data-ttu-id="81672-148">列出 4 包含公开两个方法的 Visual Basic 模块： RenderBanner() 和 RenderBannerInternal()。</span><span class="sxs-lookup"><span data-stu-id="81672-148">Listing 4 contains a Visual Basic module that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="81672-149">RenderBanner() 方法表示实际的帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="81672-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="81672-150">此方法，以便你可以在视图中就像任何其他帮助器方法一样调用 Html.RenderBanner() 扩展标准的 ASP.NET MVC HtmlHelper 类。</span><span class="sxs-lookup"><span data-stu-id="81672-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="81672-151">RenderBanner() 方法调用将 RenderBannerInternal() 方法传递给 WriteSubsitution() 方法 HttpResponse.WriteSubstitution() 方法。</span><span class="sxs-lookup"><span data-stu-id="81672-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="81672-152">RenderBannerInternal() 方法是私有方法。</span><span class="sxs-lookup"><span data-stu-id="81672-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="81672-153">此方法不会公开为一个帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="81672-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="81672-154">RenderBannerInternal() 方法随机从三个横幅播发映像的列表中返回一个横幅播发图像。</span><span class="sxs-lookup"><span data-stu-id="81672-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="81672-155">已修改的索引视图中列出 5 演示了如何使用 RenderBanner() 帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="81672-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="81672-156">请注意，其他&lt;%@ 导入 %&gt;指令是包含要导入 MvcApplication1.Helpers 命名空间的视图的顶部。</span><span class="sxs-lookup"><span data-stu-id="81672-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="81672-157">如果你忘记导入此命名空间，然后 RenderBanner() 方法不会出现作为 Html 属性的方法。</span><span class="sxs-lookup"><span data-stu-id="81672-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="81672-158">**列出 5 – Views\Home\Index.aspx （使用 RenderBanner() 方法）**</span><span class="sxs-lookup"><span data-stu-id="81672-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

<span data-ttu-id="81672-159">不同横幅播发时请求列出 5 中的视图由呈现页时，会显示每个请求 （请参见图 2）。</span><span class="sxs-lookup"><span data-stu-id="81672-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="81672-160">对页进行缓存，但横幅播发动态注入 RenderBanner() 帮助器方法。</span><span class="sxs-lookup"><span data-stu-id="81672-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="81672-161">**图 2 – 显示随机横幅播发的索引视图**</span><span class="sxs-lookup"><span data-stu-id="81672-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="81672-163">摘要</span><span class="sxs-lookup"><span data-stu-id="81672-163">Summary</span></span>

<span data-ttu-id="81672-164">本教程介绍如何动态更新中缓存的页面内容。</span><span class="sxs-lookup"><span data-stu-id="81672-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="81672-165">您学习了如何使用 HttpResponse.WriteSubstitution() 方法来使动态内容能够注入到缓存的页面。</span><span class="sxs-lookup"><span data-stu-id="81672-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="81672-166">你还了解了如何封装对 HTML 帮助程序方法内 WriteSubstitution() 方法的调用。</span><span class="sxs-lookup"><span data-stu-id="81672-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="81672-167">充分利用缓存尽可能 – 它可以对 web 应用程序的性能产生极大影响。</span><span class="sxs-lookup"><span data-stu-id="81672-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="81672-168">在本教程中所述，你可以利用的缓存甚至当你需要在页面中显示动态内容时。</span><span class="sxs-lookup"><span data-stu-id="81672-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="81672-169">[上一页](improving-performance-with-output-caching-vb.md)
[下一页](creating-a-controller-vb.md)</span><span class="sxs-lookup"><span data-stu-id="81672-169">[Previous](improving-performance-with-output-caching-vb.md)
[Next](creating-a-controller-vb.md)</span></span>