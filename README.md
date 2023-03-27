<h1 align="center">tSimpleJSON</h1>

  <p align="center">
    Talend components to generate and read JSON strings
  </p>

<br>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>

<br>

<!-- ABOUT THE PROJECT -->
## About The Project

These Talend components are designed for the easy manipulation of JSON strings. More specifically, tSimpleWriteJSON generates a JSON string from an input row and tSimpleReadJSON performs the reverse opration, that is it populates a row from an input JSON string.

<br>

<!-- GETTING STARTED -->
## Getting Started

### Prerequisites

* Talend Open Studio for Data Integration.
To install the studio, please refer to the [Talend documentation](https://help.talend.com/r/en-US/8.0/studio-getting-started-guide-open-studio-for-data-integration/introduction).


### Installation

1. Copy the tSimpleWriteJSON and tSimpleReadJSON folders into the custom components folder of your studio.
2. Open  ```Preferences -> Talend -> Components``` in your studio.
3. Click on ```Apply and Close```.
4. tSimpleWriteJSON and tSimpleReadJSON should now be available in the **JSON folder of your palette**.

> If installation via the studio fails, you can copy the folders directly into the ```[studio]/plugins/org.talend.designer.components.localprovider_[version]/components``` folder.


<br>

<!-- USAGE EXAMPLES -->
## Usage

tSimpleWriteJSON generates a JSON string from an input row. The component is mostly useful to generate a single-level JSON string that is to be sent as the body of an HTTP request, usually via the tRESTClient component.
> The component does not depend on external JSON libraries such as Jackson, which precludes problems that might arise due to class conflicts during update or migration.

<br>

tSimpleReadJSON populates a row from an input JSON string. The component is mostly useful to extract data from a single-level JSON string response to an HTTP request.

<br>

_For more info, please refer to the [Documentation](https://github.com/clanc9/tsimplejson/tree/main/doc)_

<br>


<!-- CONTRIBUTING -->
## Contributing

If you have a suggestion to improve these components, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Any contributions are welcome and appreciated.


<br>

<!-- LICENSE -->
## License

Distributed under the Apache License version 2.0. See `LICENSE.txt` for more information.

<br>


<!-- CONTACT -->
## Contact

Christian Lanctôt - info@clanctot.com

Project Link: [https://github.com/clanc9/tsimplejson](https://github.com/clanc9/tsimplejson)

<br>


<!-- ACKNOWLEDGMENTS -->
## Acknowledgments

* [Othneil Drew](https://github.com/othneildrew/Best-README-Template)
for the template of this README file.


<p align="right">(<a href="#readme-top">back to top</a>)</p>
