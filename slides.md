---
# You can also start simply with 'default'
theme: light-icons
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: 
# some information about your slides (markdown enabled)
title: Kata Postman Performance
info: |
  ## Kata Postman Performance
  Based on the Pix project, the aime of this kata will be to see how to do automated and randomized performance tests using Postman.

  Powered by [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# https://sli.dev/guide/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations#slide-transitions
transition: slide-up
# enable MDC Syntax: https://sli.dev/guide/syntax#mdc-syntax
mdc: true
---

# Kata Postman <br> To avoid a _kata_-strophe

From a simple swagger.json to a benchmark.

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
hideInToc: true
---

# What are you going to learn today ?

This kata will cover how to

* Turn a swagger into a Postman collection
* Create reusable variables and environments for your requests
* Pre-authenticate your requests thanks to the "pre-request scripts"
* Run a randomized performance tests and fine-tune its scenario
* Compare performance tests runs between them

<br>

<notes>This kata has been constructed using the <a href="https://github.com/1024pix/pix/">Pix project</a> sources.<br>Nonetheless, each steps can be followed using any other API endpoints.
</notes>

---
hideInToc: true
---

# What is expected of the participants

* **Practice, practice, practice**, in order to go through the steps of this Kata
* Sharing (and caring) among participants is strongly encouraged (ideas, tips, discoveries)
* [Law of Two feet](https://www.agilecentre.com/resources/article/meetings-with-feet/) applies

<br>

<span class="success">Ready ? <br/><span v-click >Let's go !<i class="light-icon-rocket"></i></span></span>

---
hideInToc: true
---

# Steps by steps

<br>

<Toc minDepth="1" maxDepth="1"></Toc>

---
layout: center
---

# From a swagger to a Postman Collection

---
layout: image-right
image: './images/import-collection.png'
equal: false
---

## Your Swagger as a Postman collection

**GOAL:** import a swagger into Postman for a speedy setup

Why create manually a Postman collection when your project already do the job for you ?

* First you need a Swagger definition
  * For the Pix project, it is under the endpoint [api.pix.fr/api/documentation](https://api.pix.fr/api/documentation)
* Load it effortlessly into [Postman <i class="light-icon-external-link"></i>](https://learning.postman.com/docs/designing-and-developing-your-api/importing-an-api/)
* **This step is OK if your Postman now looks alike the image**

---
layout: image-right
image: './images/feature-toggles.png'
equal: false
---

## Discover variables

**GOAL:** edit default Collection variables

Depending on your Swagger, default variables are provided

* Try [running a request](https://learning.postman.com/docs/sending-requests/requests/) without authentication, like <pre>Collection / feature-toggles / get Featuretoggles</pre>
* <span class="error">Is it working ?</span> Can you find where the main **base url** is defined ?
  * <Hint url='https://learning.postman.com/docs/sending-requests/variables/variables/#defining-collection-variables' />
* Try setting a variable so that you can make a request run successfully with value:
  * `https://api.pix.fr/api`
* Can you name the **six levels** of variables in Postman ?
  * <Hint url='https://learning.postman.com/docs/sending-requests/variables/variables/#variable-scopes' />

---
layout: image-right
image: './images/create-environment.png'
equal: false
---

## Discover environments #1

**GOAL:** create on-demande sets of variables and use it in your requests

* Create an environment named "Integration"
  * <Hint url='https://learning.postman.com/docs/sending-requests/variables/managing-environments/#create-an-environment' />
* Add an environment variable named "baseUrl" with value
  * `https://api.integration.pix.fr/api`
* Try <b>re-</b>running the feature toggle request
  * <span class="success">show that it uses the new baseUrl</span>
---
layout: image-right
image: './images/secrets.png'
equal: false
---

## Discover environments #2

**GOAL:** we can hide secret variables

* Create in your **Integration** env now the following variables
  * currentUser (default), currentPassword **(secret)**, currentScope (default)
  * Fill in with some random values
  * <span class="success">Is the currentPassword masked ?</span>
* Create in **globals** variables the following variable
  * BEARER_TOKEN with default value PREREQUEST_SCRIPT
* Your config should now look like the image

---
layout: image-right
image: './images/certification-point-of-contacts-KO.png'
equal: false
---

## Discover environments #3

**GOAL:** use a global variable in your Authorization header

* Find the Authorization tab at the top level of your Collection
  * <Hint url='https://learning.postman.com/docs/sending-requests/authorization/specifying-authorization-details/#inherit-authorization' />
  * Set it up to **Type** 'Bearer token' and the **Token** to `{{BEARER_TOKEN}}`
* Try running the 'certification-point-of-contacts' request of your Collection
  * <span class="error">Is it working ?</span>
  * Note: if error message is "Missing authentication" your BEARER_TOKEN is not properly setup

---

## Discover pre-request scripts #1

TODO: to complete

````md magic-move {lines: true}
```js {*|2|12|13|14}
pm.sendRequest({
    url: pm.environment.get("baseUrl")+"/token",
    method: 'POST',
    header: {
        'Accept': 'application/json',
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    body: {
        mode: 'urlencoded',
        urlencoded: [
            {key: 'grant_type', value: 'password'},
            {key: 'scope', value: pm.environment.get("currentScope")},
            {key: 'username', value: pm.environment.get("currentUser")},
            {key: 'password', value: pm.environment.get("currentPassword")}
        ]
    }
},
    (err, res) => {
        // Set BEARERTOKEN
        pm.globals.set("BEARERTOKEN", res.json().access_token)
        // console.log(res.json());
});
```
````

---
---

## Discover pre-request scripts #2
