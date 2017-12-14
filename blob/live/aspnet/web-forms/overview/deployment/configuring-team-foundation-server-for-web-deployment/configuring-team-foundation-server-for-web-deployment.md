---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: "用于 Web 部署配置 Team Foundation Server |Microsoft 文档"
author: jrjlee
description: "本教程将演示如何配置 Team Foundation Server (TFS) 2010年若要生成解决方案，并将 web 内容部署到各种目标环境。 这..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 72f60841a1381380c0ea6167077420f960180dc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2017
---
<a name="configuring-team-foundation-server-for-web-deployment"></a><span data-ttu-id="55fc8-104">用于 Web 部署配置 Team Foundation Server</span><span class="sxs-lookup"><span data-stu-id="55fc8-104">Configuring Team Foundation Server for Web Deployment</span></span>
====================
<span data-ttu-id="55fc8-105">通过[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="55fc8-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="55fc8-106">下载 PDF</span><span class="sxs-lookup"><span data-stu-id="55fc8-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="55fc8-107">本教程将演示如何配置 Team Foundation Server (TFS) 2010年若要生成解决方案，并将 web 内容部署到各种目标环境。</span><span class="sxs-lookup"><span data-stu-id="55fc8-107">This tutorial will show you how to configure Team Foundation Server (TFS) 2010 to build solutions and deploy web content to various target environments.</span></span> <span data-ttu-id="55fc8-108">这包括持续集成 (CI) 方案中，在其中部署内容自动每次一名开发人员进行了更改。</span><span class="sxs-lookup"><span data-stu-id="55fc8-108">This includes continuous integration (CI) scenarios, where you deploy content automatically every time a developer makes a change.</span></span> <span data-ttu-id="55fc8-109">它还包括手动触发器方案中，管理员可能想要验证并在测试环境中验证生成之后触发部署到过渡环境特定生成的位置。</span><span class="sxs-lookup"><span data-stu-id="55fc8-109">It can also include manual trigger scenarios, where an administrator may want to trigger deployment of a specific build to a staging environment once the build has been verified and validated in the test environment.</span></span> <span data-ttu-id="55fc8-110">在本教程中的主题将指导你完成整个配置过程中，包括：</span><span class="sxs-lookup"><span data-stu-id="55fc8-110">The topics in this tutorial will guide you through the entire configuration process, including:</span></span>
> 
> - <span data-ttu-id="55fc8-111">如何在 TFS 中创建新的团队项目。</span><span class="sxs-lookup"><span data-stu-id="55fc8-111">How to create a new team project in TFS.</span></span>
> - <span data-ttu-id="55fc8-112">如何将内容添加到源代码管理。</span><span class="sxs-lookup"><span data-stu-id="55fc8-112">How to add content to source control.</span></span>
> - <span data-ttu-id="55fc8-113">如何配置生成服务器以支持 CI 和部署。</span><span class="sxs-lookup"><span data-stu-id="55fc8-113">How to configure a build server to support CI and deployment.</span></span>
> - <span data-ttu-id="55fc8-114">如何创建生成定义，其中包含部署逻辑。</span><span class="sxs-lookup"><span data-stu-id="55fc8-114">How to create a build definition that includes deployment logic.</span></span>
> - <span data-ttu-id="55fc8-115">如何配置用于自动部署的权限。</span><span class="sxs-lookup"><span data-stu-id="55fc8-115">How to configure permissions for automated deployment.</span></span>
> 
> <span data-ttu-id="55fc8-116">这些教程的意大利语翻译，请访问[http://www.lucamorelli.it](http://www.lucamorelli.it)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-116">For an Italian translation of these tutorials, visit [http://www.lucamorelli.it](http://www.lucamorelli.it).</span></span>


<span data-ttu-id="55fc8-117">本教程假定你已安装 TFS 2010 并创建团队项目集合作为初始配置过程的一部分。</span><span class="sxs-lookup"><span data-stu-id="55fc8-117">This tutorial assumes that you have installed TFS 2010 and created a team project collection as part of the initial configuration process.</span></span> <span data-ttu-id="55fc8-118">[For Visual Studio 2010 Team Foundation 安装指南](https://go.microsoft.com/?linkid=9805132)提供全面指导，这些任务。</span><span class="sxs-lookup"><span data-stu-id="55fc8-118">The [Team Foundation Installation Guide for Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) provides comprehensive guidance on these tasks.</span></span>

## <a name="context"></a><span data-ttu-id="55fc8-119">上下文</span><span class="sxs-lookup"><span data-stu-id="55fc8-119">Context</span></span>

<span data-ttu-id="55fc8-120">此窗体的基于名为 Fabrikam，Inc.的虚构公司的企业部署要求的教程系列中的一部分本系列教程使用的示例解决方案 （&） #x 2014;[联系人管理器](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)来表示具有现实级别的复杂性，包括 ASP.NET MVC 3 应用程序，Windows 的 web 应用程序解决方案 （&) #x 2014;Communication Foundation (WCF) 服务和数据库项目。</span><span class="sxs-lookup"><span data-stu-id="55fc8-120">This forms part of a series of tutorials based on the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="55fc8-121">这些教程的核心的部署方法取决于中介绍的拆分项目文件方法[了解该生成过程](../web-deployment-in-the-enterprise/understanding-the-build-process.md)，在其中生成过程控制由两个项目文件 （&） #x 2014; 一个包含生成适用于每种目标环境和一个包含特定于环境的生成和部署设置的说明。</span><span class="sxs-lookup"><span data-stu-id="55fc8-121">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="55fc8-122">在生成期间，特定于环境的项目文件合并到环境无关的项目文件中以形成一组完整的生成说明。</span><span class="sxs-lookup"><span data-stu-id="55fc8-122">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="scenario-overview"></a><span data-ttu-id="55fc8-123">方案概述</span><span class="sxs-lookup"><span data-stu-id="55fc8-123">Scenario Overview</span></span>

<span data-ttu-id="55fc8-124">这些教程的高级方案所述[企业 Web 部署： 方案概述](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-124">The high-level scenario for these tutorials is described in [Enterprise Web Deployment: Scenario Overview](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).</span></span> <span data-ttu-id="55fc8-125">我们建议您开始本教程之前查看此主题。</span><span class="sxs-lookup"><span data-stu-id="55fc8-125">We recommend that you review this topic before you get started on this tutorial.</span></span>

## <a name="how-to-use-this-tutorial"></a><span data-ttu-id="55fc8-126">如何使用本教程</span><span class="sxs-lookup"><span data-stu-id="55fc8-126">How to Use This Tutorial</span></span>

<span data-ttu-id="55fc8-127">如果这是首次执行了本教程中所述的任务，或如果你想要遵照使用的示例解决方案的示例，你应解决顺序中的教程主题。</span><span class="sxs-lookup"><span data-stu-id="55fc8-127">If this is the first time you've performed the tasks described in this tutorial, or if you want to follow the examples using the sample solution, you should work through the tutorial topics in order.</span></span> <span data-ttu-id="55fc8-128">或者，可以使用单个主题作为指导特定任务。</span><span class="sxs-lookup"><span data-stu-id="55fc8-128">Alternatively, you can use individual topics as guidance for specific tasks.</span></span> <span data-ttu-id="55fc8-129">本教程包括以下主题：</span><span class="sxs-lookup"><span data-stu-id="55fc8-129">This tutorial includes these topics:</span></span>

- <span data-ttu-id="55fc8-130">[在 TFS 中创建团队项目](creating-a-team-project-in-tfs.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-130">[Creating a Team Project in TFS](creating-a-team-project-in-tfs.md).</span></span> <span data-ttu-id="55fc8-131">团队项目是源代码管理、 进程管理，以及在 TFS 中的生成的核心单元。</span><span class="sxs-lookup"><span data-stu-id="55fc8-131">A team project is the core unit for source control, process management, and build in TFS.</span></span> <span data-ttu-id="55fc8-132">你需要创建团队项目，然后才能添加要源代码管理或创建生成定义的内容。</span><span class="sxs-lookup"><span data-stu-id="55fc8-132">You need to create a team project before you can add content to source control or create build definitions.</span></span>
- <span data-ttu-id="55fc8-133">[将内容添加到源代码管理](adding-content-to-source-control.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-133">[Adding Content to Source Control](adding-content-to-source-control.md).</span></span> <span data-ttu-id="55fc8-134">一旦你已创建团队项目，即可将内容添加到源代码管理。</span><span class="sxs-lookup"><span data-stu-id="55fc8-134">Once you've created a team project, you can start adding content to source control.</span></span> <span data-ttu-id="55fc8-135">你将需要之前要添加的项目和解决方案，以及任何外部依赖关系，你可以开始配置生成。</span><span class="sxs-lookup"><span data-stu-id="55fc8-135">You'll need to add your projects and solutions, together with any external dependencies, before you can start configuring builds.</span></span>
- <span data-ttu-id="55fc8-136">[用于 Web 部署配置 TFS 生成服务器](configuring-a-tfs-build-server-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-136">[Configuring a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span> <span data-ttu-id="55fc8-137">如果你想要生成你的团队项目内容，你将需要配置生成服务器。</span><span class="sxs-lookup"><span data-stu-id="55fc8-137">If you want to build your team project content, you'll need to configure a build server.</span></span> <span data-ttu-id="55fc8-138">在大多数情况下，这应该是从你的 TFS 安装一台计算机上。</span><span class="sxs-lookup"><span data-stu-id="55fc8-138">In most cases, this should be on a separate machine from your TFS installation.</span></span> <span data-ttu-id="55fc8-139">若要配置生成服务器，你需要安装和配置 TFS 生成服务，安装 Visual Studio 2010、 创建生成控制器和生成代理，安装任何产品或组件，你的代码需要才能成功，生成和安装Internet Information Services (IIS) Web 部署工具 （Web 部署）。</span><span class="sxs-lookup"><span data-stu-id="55fc8-139">To configure a build server, you need to install and configure the TFS build service, install Visual Studio 2010, create build controllers and build agents, install any products or components that your code needs in order to build successfully, and install the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span>
- <span data-ttu-id="55fc8-140">[创建生成定义支持部署](creating-a-build-definition-that-supports-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-140">[Creating a Build Definition That Supports Deployment](creating-a-build-definition-that-supports-deployment.md).</span></span> <span data-ttu-id="55fc8-141">在可以开始队列或触发在 TFS 中的生成之前，你需要为你的团队项目创建至少一个生成定义。</span><span class="sxs-lookup"><span data-stu-id="55fc8-141">Before you can start queuing or triggering builds in TFS, you need to create at least one build definition for your team project.</span></span> <span data-ttu-id="55fc8-142">生成定义中定义生成，包括应在生成中包括哪些内容、 应触发生成，以及其中 Team Build 应发送的生成输出每个的方面。</span><span class="sxs-lookup"><span data-stu-id="55fc8-142">The build definition defines every aspect of the build, including which things should be included in the build, what should trigger the build, and where Team Build should send the build outputs.</span></span> <span data-ttu-id="55fc8-143">你可以配置生成定义以运行自定义 Microsoft Build Engine (MSBuild) 项目文件，这样就可以自动生成中包括部署逻辑。</span><span class="sxs-lookup"><span data-stu-id="55fc8-143">You can configure a build definition to run custom Microsoft Build Engine (MSBuild) project files, which lets you include deployment logic in your automated builds.</span></span>
- <span data-ttu-id="55fc8-144">[将特定的生成部署](deploying-a-specific-build.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-144">[Deploying a Specific Build](deploying-a-specific-build.md).</span></span> <span data-ttu-id="55fc8-145">在许多方案，你将想要将特定生成，而不是最新的生成、 部署到目标环境。</span><span class="sxs-lookup"><span data-stu-id="55fc8-145">In a lot of scenarios, you'll want to deploy a specific build, rather than the latest build, to a target environment.</span></span> <span data-ttu-id="55fc8-146">在这种情况下，你可以配置将特定的放置文件夹中的内容部署的生成定义。</span><span class="sxs-lookup"><span data-stu-id="55fc8-146">In this case, you can configure a build definition that deploys content from a specific drop folder.</span></span>
- <span data-ttu-id="55fc8-147">[配置的团队生成权限部署](configuring-permissions-for-team-build-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-147">[Configuring Permissions for Team Build Deployment](configuring-permissions-for-team-build-deployment.md).</span></span> <span data-ttu-id="55fc8-148">如果生成服务是将内容部署自动化的生成过程的一部分，你需要授予给任何目标 web 服务器和数据库服务器上的生成服务帐户不同的权限。</span><span class="sxs-lookup"><span data-stu-id="55fc8-148">If the build service is to deploy content as part of an automated build process, you need to grant various permissions to the build service account on any destination web servers and database servers.</span></span>

## <a name="key-technologies"></a><span data-ttu-id="55fc8-149">主要的技术</span><span class="sxs-lookup"><span data-stu-id="55fc8-149">Key Technologies</span></span>

<span data-ttu-id="55fc8-150">本教程重点介绍如何使用这些产品和技术以支持自动的生成和 web 部署：</span><span class="sxs-lookup"><span data-stu-id="55fc8-150">This tutorial focuses on how to use these products and technologies to support automated build and web deployment:</span></span>

- <span data-ttu-id="55fc8-151">Visual Studio Team Foundation Server 2010</span><span class="sxs-lookup"><span data-stu-id="55fc8-151">Visual Studio Team Foundation Server 2010</span></span>
- <span data-ttu-id="55fc8-152">Team Build 和 MSBuild</span><span class="sxs-lookup"><span data-stu-id="55fc8-152">Team Build and MSBuild</span></span>
- <span data-ttu-id="55fc8-153">Web Deploy</span><span class="sxs-lookup"><span data-stu-id="55fc8-153">Web Deploy</span></span>

<span data-ttu-id="55fc8-154">本教程还涉及使用 Windows Server 2008 R2、 IIS 7.5、 SQL Server 2008 R2、 ASP.NET 4.0 和 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="55fc8-154">The tutorial also touches on the use of Windows Server 2008 R2, IIS 7.5, SQL Server 2008 R2, ASP.NET 4.0, and ASP.NET MVC 3.</span></span>

## <a name="other-tutorials-in-this-series"></a><span data-ttu-id="55fc8-155">在本系列中其他教程</span><span class="sxs-lookup"><span data-stu-id="55fc8-155">Other Tutorials in This Series</span></span>

<span data-ttu-id="55fc8-156">它在企业级 web 部署构成的五个教程系列中的一部分。</span><span class="sxs-lookup"><span data-stu-id="55fc8-156">This forms part of a series of five tutorials on enterprise-scale web deployment.</span></span> <span data-ttu-id="55fc8-157">这些是序列中的其他教程：</span><span class="sxs-lookup"><span data-stu-id="55fc8-157">These are the other tutorials in the series:</span></span>

- <span data-ttu-id="55fc8-158">[部署在企业方案中的 Web 应用程序](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-158">[Deploying Web Applications in Enterprise Scenarios](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md).</span></span> <span data-ttu-id="55fc8-159">本介绍性内容的系列教程提供上下文的背景。</span><span class="sxs-lookup"><span data-stu-id="55fc8-159">This introductory content provides the contextual background for the tutorial series.</span></span> <span data-ttu-id="55fc8-160">描述该教程的方案，以及它阐释了任务和在整个系列所述的演练如何适应更广泛的应用程序生命周期管理 (ALM) 过程。</span><span class="sxs-lookup"><span data-stu-id="55fc8-160">It describes the tutorial scenario, and it illustrates how the tasks and walkthroughs described throughout the series fit into a broader Application Lifecycle Management (ALM) process.</span></span>
- <span data-ttu-id="55fc8-161">[Web 部署在企业中的](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-161">[Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="55fc8-162">本教程提供 MSBuild 项目文件的概念介绍、 Web 发布管道 (WPP)、 Web 部署和其他相关的技术。</span><span class="sxs-lookup"><span data-stu-id="55fc8-162">This tutorial provides a conceptual introduction to MSBuild project files, the Web Publishing Pipeline (WPP), Web Deploy, and other related technologies.</span></span> <span data-ttu-id="55fc8-163">它还说明了如何使用这些工具在一起来管理复杂部署进程。</span><span class="sxs-lookup"><span data-stu-id="55fc8-163">It explains how you can use these tools together to manage complex deployment processes.</span></span>
- <span data-ttu-id="55fc8-164">[用于 Web 部署配置服务器环境](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-164">[Configuring Server Environments for Web Deployment](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).</span></span> <span data-ttu-id="55fc8-165">本教程介绍如何配置 Windows server 以支持各种部署方案，包括使用 Web 部署代理服务 （远程代理） 或 Web 部署处理程序和远程数据库部署的远程 web 包部署。</span><span class="sxs-lookup"><span data-stu-id="55fc8-165">This tutorial describes how to configure Windows servers to support various deployment scenarios, including remote web package deployment using the Web Deployment Agent Service (the remote agent) or the Web Deploy Handler and remote database deployment.</span></span> <span data-ttu-id="55fc8-166">它提供有关选择你自己的环境的适当的部署方法的指导，并描述了如何使用 Web Farm Framework (WFF) 在服务器场中的所有 web 服务器上复制已部署的 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="55fc8-166">It provides guidance on choosing the appropriate deployment method for your own environment, and it describes how to use the Web Farm Framework (WFF) to replicate deployed web applications across all the web servers in a server farm.</span></span>
- <span data-ttu-id="55fc8-167">[高级企业级 Web 部署](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md)。</span><span class="sxs-lookup"><span data-stu-id="55fc8-167">[Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md).</span></span> <span data-ttu-id="55fc8-168">本教程介绍如何完成各种更高级的部署任务，如自定义数据库部署为多个环境、 从部署中排除文件和文件夹以及在部署过程中将 web 应用程序脱机.</span><span class="sxs-lookup"><span data-stu-id="55fc8-168">This tutorial describes how to accomplish various more advanced deployment tasks, like customizing database deployments for multiple environments, excluding files and folders from deployment, and taking web applications offline during the deployment process.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="55fc8-169">下一篇</span><span class="sxs-lookup"><span data-stu-id="55fc8-169">Next</span></span>](creating-a-team-project-in-tfs.md)