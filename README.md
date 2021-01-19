Google Tag Manager tech specs for Portail Auto-Entrepreneur

# Google Tag Manager initialization

GTM-565LS2J container should be called on all pages : 

- Either in the ```<head>``` div of each page

```html
<!-- Google Tag Manager -->
<script>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-565LS2J');</script>
<!-- End Google Tag Manager -->
```

- Or through a package such as [NPM Google Tag Manager plugin for analytics](https://www.npmjs.com/package/@analytics/google-tag-manager)

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
# Previous tags cleaning

All previous hardcoded Google Analytics / Tag Manager snippets should be removed from the code, as they will be dealt with through GTM directly

# Incorp funnel steps

For each Incorporate step, user infos should be displayed

```javascript
dataLayer.push({
    'event' : 'incorpStep',
    'incorpStepName' : 'Activité', //Step name, identical to what is displayed : 'Déclarant', 'Activité', 'Adresse', 'Règlement', 'Finalisation'
    'incorpUserInfos' : { //User infos displayed in the previous step that was validated

        //Infos to display when step 2 is loaded
        'gender' : 'male', //'male' or 'female'
        'birthCountry' : 'Abroad', //'Abroad' or 'France'  
        'birthYear' : 1988, //AAAA format
        'nationality' : 'France', //Self explanatory

        //Infos to display when step 3 is loaded
        'activityStartDate' : '20210101', //Company start date, YYYYMMDD format
        'activityField' : 'Cours et formation | Cours à domicile', //Concatenates "Domaine d'activité" and "Activité souhaitée" fields

        //Infos to display when step 4 is loaded
        'city' : 'RENNES (35000)', //City and post code as displayed on the page

        //Infos to display when step 5 is loaded
        'transactionAmount' : 59 //Transaction amount in €        
    }
})
```

# Editorial content loading

Each time a 'actualite' or 'academie' article is loaded : 

```javascript
dataLayer.push({
    'event' : 'editoContentLoad',
    'editoBreadcrumb' : ['Académie','Gérer son auto-entreprise','Modifier son auto-entreprise','L\'ajout d\'activité en auto-entreprise'],
    //All breadcrumb items, as displayed on the page
    'editoTags' : '', //To be validated by Solen
    'editoChars' : 4321, //Number of characters in the article
    'editoPublishDate' : '1611052308', //Unix Timestamp for the article publication date 
})
```

# Editorial content HTML marking

Users rating block should be decorated the following way :

![Drag Racing](https://i.ibb.co/CwHD48S/Sans-titre.png)

```html
<footer class="rating-footer text-center w80 mauto" tms-article-footer="yes">
```

Side CtAs? ==> To be validated with Solen


# User login

```javascript
dataLayer.push({
    'event' : 'login',
    'userID' : '123456789' //Portail auto-entrepreneur User ID 
})
```
We could also use a cookie => To be validated with Solen

# Deployment notes

These specs should be first deployed on a staging environment, and in production in sync with Google Tag Manager publication managed by SouthWatts