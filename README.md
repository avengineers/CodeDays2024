## Editing and viewing locally

* Clone the repository including submodules:

```powershell
git clone --recurse-submodules https://github.com/avengineers/CodeDays2024.git
```

* For editing the slides I recommend using [Visual Studio Code](https://code.visualstudio.com/).
* For viewing you need to install and run a local webserver to view them:
  * To install the webserver, run `npm install` in the root folder of the repository or use the VS Code task `Install dependencies`.
  * To run the webserver, run `npm start` in the root folder of the repository or use the VS Code task `Run local webserver`.
  * When the webservice is running, open your browser and navigate to `http://localhost:8000/` to view the slides.
