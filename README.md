Google Tag Manager tech specs for Portail Auto-Entrepreneur

# Google Tag Manager initialization

GTM-565LS2J container should be called on all pages : 

- Either in the ```<head>``` + ```<body>```div of each page :

```html
<!-- Google Tag Manager - head snippet -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-565LS2J');</script>
<!-- End Google Tag Manager- head snippet  -->
```

```html
<!-- Google Tag Manager - body snippet -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-565LS2J"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager - body snippet  -->
```

- Or through a package such as [NPM Google Tag Manager plugin for analytics](https://www.npmjs.com/package/@analytics/google-tag-manager) :

```bash
npm install analytics
npm install @analytics/google-tag-manager
```

```javascript
import Analytics from 'analytics'
import googleTagManager from '@analytics/google-tag-manager'

const analytics = Analytics({
  app: 'awesome-app',
  plugins: [
    googleTagManager({
      containerId: 'GTM-565LS2J'
    })
  ]
})
```

Either way, To make sure basic GTM setup is valid, please use [Google Tag Assistant Chrome Plugin](https://chrome.google.com/webstore/detail/tag-assistant-legacy-by-g/kejbdjndbnbjgmefkgdddjlbokphdefk?hl=fr)
(soon to be deprecated)

When installed on a page / app where it is set, GTM installation should be validated with a green icon for GTM as following :

<img src="https://i.ibb.co/HY2rm5S/Capture-d-cran-2021-01-22-102033.png" alt="GTM validation" width="250"/>

# Previous tags cleaning

All previous hardcoded Google Analytics / Tag Manager snippets that may exist should be removed from the code, as they will be dealt with through GTM directly, and could cause issues such as double pageviews if they're kept.

# Incorp funnel steps

<img src="https://i.ibb.co/t4BGsns/Capture-d-cran-2021-01-22-102328.png" alt="Incorp" width="600"/>

When an incorp step X is validated, user infos validated in the step should be populated in the data layer :

```javascript
dataLayer.push({
    'event' : 'incorpStepValidation',
    'incorpStepName' : 'Activité', //Step name that was validated, identical to what is displayed on page : 'Déclarant', 'Activité', 'Adresse', 'Règlement', 'Finalisation'
    'incorpUserInfos' : { //User infos displayed in the step that was validated

        //Info when step 1 "Déclarant" is validated, before step 2 load
        'gender' : 'male', //'male' or 'female'
        'birthCountry' : 'Abroad', //'Abroad' or 'France'  
        'birthYear' : 1988, //AAAA format
        'nationality' : 'France', //Self explanatory

        //Info when step 2 "Activité" is validated, before step 3 load
        'activityStartDate' : '20210101', //Company start date, YYYYMMDD format
        'activityField' : {
          'activityDomain':'Cours et formation', //"Domaine d'activité" field
          'activityWish':'Cours à domicile' //"Domaine souhaité" field 
          },

        //Info when step 3 "Adresse" is validated, before step 4 load
        'city' : 'RENNES (35000)', //City and post code as displayed on the page

        //Info when step 4 "Règlement" is validated, before step 5 load
        'transactionAmount' : 59 //Transaction amount in €        
    }
})
```

# Editorial content loading

Each time an 'actualite' or 'academie' content, **article or category page** is loaded : 

```javascript
dataLayer.push({
    'event' : 'editoContentLoad',
    'editoType' : 'article', //'article' or 'category'
    'editoBreadcrumb' : ['Académie','Gérer son auto-entreprise','Modifier son auto-entreprise','L\'ajout d\'activité en auto-entreprise'],
    //All category, sub category, etc... items displayed in the same order as on the page
    'editoChars' : 4321, //Number of characters in the article, in the case of 'article' only
    'editoPublishDate' : '1611052308', //Unix Timestamp for the article publication date, in the case of 'article' only
})
```

# Editorial content HTML markup

## Users rating block

Users rating block should be marked up the following way with a ```tms-article-footer-block``` attribute  :

![User rating block](https://i.ibb.co/CwHD48S/Sans-titre.png)

```html
<footer class="rating-footer text-center w80 mauto" tms-article-footer-block="yes">
```

## Newsletter side block

Newsletter block at the side of the articles should be marked up the following way with a ```tms-newsletter-side``` attribute  :

Missing screenshot

Missing HTML markup 

## Auto entreprise creation block

"Je créé mon auto entreprise" call to action in the sticky block at the bottom of articles should be marked up the following way with a ```tms-auto-entr-bottom-block``` attribute  :

!["Je créé mon auto entreprise" sticky block](https://i.ibb.co/cvJMFDQ/Capture-d-cran-2021-01-22-095232.png)

```html
<a href="/devenir-auto-entrepreneur" class="" tms-auto-entr-bottom-block="yes">JE CRÉE MON AUTO-ENTREPRISE</a>
```

# User login

Each time there is an update to the login status in the app (login or logout)

```javascript
dataLayer.push({
    'event' : 'loginStatusUpdate',
    'loginStatusUpdateType' : 'login', //'login' or 'logout'
    'userID' : '123456789' //Portail auto-entrepreneur User ID, matching your CRM ID 
})
```

# Deployment notes

These specs should be first deployed on a staging environment, and only in production in sync with Google Tag Manager publication managed by SouthWatts

# Contact

Please reach aristide.riou@southwatts.com for any questions