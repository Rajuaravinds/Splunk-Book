node v14 or v12
yarn >= 1.2

npm install react@^16 react-dom@^16 styled-components@^5
   //package.json gernerated == SplunkUi dependencies 

npm install --global @splunk/react-ui
npm install --global yarn

yarn run setup  --- (lerna package comes here)
    //yarn.lock file generated

yarn run build

cd /packages/<app.name>/

pre://set env the path to splunk/etc/app    

setx SPLUNK_HOME "C:\Program Files\Splunk"


//admin pre req to make symlink

yarn run link:app

---------------------------
uicomp -- renders the view from the component
