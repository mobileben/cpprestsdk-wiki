# Contribution Guidelines

If you would like to become involved in the development of this SDK there are many different ways in which you can contribute. We strongly value user feedback and will appreciate your questions, bug reports, and feature requests. In addition you can also contribute changes to the code, which include bug fixes and improvements as well as new features.

### Using the product and providing feedback

Using the APIs, asking and answering questions, reporting bugs and making feature requests are critical parts of the project community. User feedback is crucial for improving the quality of the products and shaping further development. In order to become familiar with the functionality you can download the pre-compiled binaries or sync the source code from CodePlex and compile locally. Once you become familiar with the functionality you can report bugs or request new features.  
<a name="questions"></a>

### Report bugs and request features

Issues and feature requests are submitted through the project's [issue tracker](../issues) section on CodePlex. Please use the following guidelines when you submit issues and feature requests:

*   Make sure the issue is not already reported by searching through the list of issues
*   Provide a detailed description of the issue including the following information:
    *   Which feature the issue appears in
    *   Under what circumstances the issue appears
    *   What is the desired behavior
    *   What is breaking
    *   What is the impact (things like loss or corruption of data, compromising security, disruption of service etc.)
    *   Any code that will be helpful to reproduce the issue

Issues are regularly reviewed and updated with additional information by the team. Sometimes we may have questions about particular issue that might need clarifications so, please be ready to provide additional contact information.  
<a name="contribute"></a>

## How to become a contributor?

In order to become a contributor to the project we need you to sign the [Contributor License Agreement (CLA)](https://www.codeplex.com/Download?ProjectName=casablanca&DownloadId=623578). Signing the Contributor License Agreement (CLA) does not grant you rights to commit to the main repository but it does mean that we will consider your contributions and you will get credit if we do. Please fill in, sign, scan, and email it to [askcasablanca@microsoft.com](mailto:askcasablanca@microsoft.com). Please note if you've signed another CLA with Microsoft then that should be fine as well. Just let us know.  

### Create bug fixes and features

You make modifications of the code in your local Git repository. Once you are done with your implementation follow the steps below:

*   Submit the changes to your own fork by using the following command <span class="codeInline">git submit</span>
*   In CodePlex create new pull request to the development branch by clicking on the Pull Request button
*   Write detailed message describing the changes in the pull request
*   Submit the pull request for consideration by the team

_Note: Please keep in mind that not all requests will be approved. Requests are reviewed by the team on a regular basis and will be updated with the status at each review. If your request is accepted you will receive information about the next steps and when the request will be integrated in the development branch. If your request is rejected you will receive information about the reasons why it was rejected._  

### Contribution guidelines

Before you start working on bug fixes and features it is good idea to discuss those broadly with the community. You can use the [issue tracker](../issues) for this purpose. Before submitting your changes make sure you followed the guidelines below:

*   You have properly documented any new functionality
*   For any new functionality you have written complete unit tests
*   You have run all unit tests and they pass
*   In order to speed up the process of accepting your contributions, you should try to make your checkins as small as possible, avoid any unnecessary deltas and the need to rebase.