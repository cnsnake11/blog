# 【翻译】How TensorFlow Lite Brings Machine Learning to Mobile Devices

# TensorFlow Lite --- 赋予移动设备机器学习的能力

原文地址： http://www.tomsitpro.com/articles/tensorflow-light-machine-learning-mobile,1-3670.html

Android developers will appreciate TensorFlow Lite, which promises to give them machine learning tools for mobile apps.

安卓的开发者将会使用到TensorFlow Lite用于移动设备机器学习的开发。

![](media/15090124219258.jpg)

Machine learning requires incredibly complex computing power and resources. While research and artificial intelligence breakthroughs gain most of the attention, the most dramatic difference in daily computing happens on mobile devices. It's smartphones where the user is typically interacting with information that is personal, and would benefit from a computer that anticipates one's needs.

机器学习需要大量的计算资源。伴随着人工智能领域研究的突破，移动设备的计算方式也在发生着巨大的变化。我们通过智能手机获取信息的方式，可以从计算机的智能预测需求中获益。

Businesses are counting on machine learning to enhance productivity and do things that were previously impossible. Google recently offered some insights into how the company wants to affect change with its own mobile operating system.

商业可以利用机器学习提高生产力，并且做到之前做不到的事情。谷歌最近提出了一些观点：公司如何改善自己的移动操作软件。

Android developers should keep an eye on TensorFlow Lite, which is promised to give them machine learning tools for building such smarts into their own mobile applications. It's an optimized version of TensorFlow, which is Google's popular open-source library that enables researchers and developers to apply machine learning to their applications.

安卓开发者应该时刻关注TensorFlow Lite，它会为应用带来机器学习的能力。TensorFlow Lite是TensorFlow的优化版，TensorFlow是谷歌的开源机器学习框架。

The news about TensorFlow Lite was first announced at Google I/O, where the company illustrated the potentials for machine learning on mobile devices by showing off how Google Lens will be able to recognize objects and infuse artificial intelligence in other ways into image recognition. At I/O and in other announcements, Google has made a point of casting itself as an "AI-first" company.

TensorFlow Lite是在谷歌IO大会行公布的，大会上演示了Google Lens的图片识别及人工智能技术。google已经宣称自己是AI-first公司。

# Arrival of new APIs 新的api

While developers await the arrival of TensorFlow Lite, they can look to some new APIs rolling out to the standard version of TensorFlow for inspiration. While it's not a direct indication of what the Lite model will look like, they exhibit further insight into what TensorFlow and its machine learning capabilities make possible.

在开发者等待TensorFlow Lite正式发布的过程中，可以先调研一下基于TensorFlow衍生出来的心api，这会带来新的灵感。这虽然不能看出TensorFlow Lite的模型是什么样的，但却能看出未来机器学习的潜力都是什么。

Google recently also release a new Object Detection API to assist those working with computer vision models. As outlined in a Google Research blog post, the new set of APIs enables researchers to create more complex object detection. An example of TensorFlow at work illustrates how machine learning enables recognition of kites and people from the following scene.

google最近发布了对象检测api，他可以为视觉分析提供帮助。Google Research blog概括了一下，新的api可以让开发者对复杂的对象进行识别。下边的场景会说明TensorFlow是怎样应用到机器学习中进行图形识别的。

"This codebase is an open-source framework built on top of TensorFlow that makes it easy to construct, train and deploy object detection models. Our goals in designing this system was to support state-of-the-art models while allowing for rapid exploration and research," wrote research scientist Jonathan Huang and software engineer Vivek Rathod on the Google Research blog.

“这个代码库是基于TensorFlow开发的开源框架，它可以很简单进行构造，训练，部署对象检测模型。我们设计这个系统的目标是去支持顶尖的模型，来进行快速的研究工作”，开发者们说。

Google, of course, isn't the only one offering machine learning resources to developers. Facebook, Apple and Microsoft have been building their own resources. Facebook's Caffe2Go framework has its own models that can run on smartphones. Apple's Core ML is built for running machine learning on iOS. Microsoft Azure also offers machine learning as a service.

Google并不是唯一一家提供机器学习开发资源的公司。facebook，apple，微软，都在建设他们自己的资源。facebook的Caffe2Go框架也已经可以运行在智能手机上了。苹果的Core ML是ios上运行机器学习的解决方案。Microsoft Azure也提供了机器学习作为一个服务。

The capabilities are growing quickly, as illustrated by the current release of the TensorFlow APIs. The ongoing challenge of performing machine learning on device is the resource constraints inherent to mobile computing. While doing such computing on the device itself enhances privacy and in some cases may be more efficient, much of the work must still be performed in the cloud.

目前版本的TensorFlow的功能增长非常快。在移动设备上运行机器学习的最大挑战是硬件资源。有时候需要设备有非常快的运算速度，所以部分工作可能还是需要在云端处理。

Also at I/O, Google also promised that Android O would include a framework for more accelerated neural computation. Google, Apple and other companies that make devices will also have to think about machine learning as they design their hardware and software. SSD models are lightweight enough, for example, to run in real-time on mobile devices. But as phones become always-aware, intelligent assistants, the role of machine learning in everything from at the system level to third-party applications will become larger.

在IO大会上，谷歌也表示在android O上，会内置性能很高的神经网络计算框架。谷歌，苹果等公司已经开始去考虑如何设计软件硬件来支持机器学习。SSD模型就是一个轻量级的即时运算的机器学习模型，可以运行在移动设备上。随着手机越来越智能，机器学习将会在系统级别和第三方应用中大展宏图。


