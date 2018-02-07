/*	
	The following code has been referenced from the code
	distributed by Microsoft Azure API. All code and comments 
	are credited to the API repository resource guide.

	All code and variable declarations can be found at:
	https://msdn.microsoft.com/en-us/library/ff512421.aspx

	For personal use, I have modified some methods and comments 
	to be applied to the project's use case.
*/

// local variables for web interface
var allLanguageCodes = ''; 
var allLanguageNames = '';

// required declarations for API
var codeToLanguage = {};
var languageToCode = {};

// Initiate handshake with API
function callAPI(api)
{
	var scriptcall = document.createElement("script");
	scriptcall.type = 'text/javascript';
	scriptcall.src = api;
	document.getElementsByTagName("head")[0].appendChild(scriptcall);
}


function onTranslate(message)
{
	document.getElementById("destTextArea").value = message;
}

// Call the API to translate desired text using chosen languages
function translate()
{
	sourceLanguage = languageToCode[document.getElementById('fromLang').value];
	destinationLanguage = languageToCode[document.getElementById('toLanguageSelect').value];
	translatedMessage = document.getElementById("srcTextArea").value;
	sendToAPI = "http://api.microsofttranslator.com/V2/Ajax.svc/Translate?oncomplete=onTranslate&appId=86B37AB482F841F7A33D2AB16430EF7C46E8775F&from=" + sourceLanguage + "&to=" + destinationLanguage + "&text=" + translatedMessage;
	
	callAPI(sendToAPI);
}

// Gather text to be translated and store locally
function onDetect(message)
{
	if(document.getElementById("srcTextArea").value != '')
	{
		if(codeToLanguage[message] == undefined)
		{
			document.getElementById("fromLang").value = 'not available';
			document.getElementById("languageAvailableText").innerHTML = 'See the dropdown above for a list of languages available for translation';
			document.getElementById("destTextArea").value = '';
		}
		else
		{
			document.getElementById("fromLang").value = codeToLanguage[message];
			document.getElementById("languageAvailableText").innerHTML = '  ';
		}
	}
	if( document.getElementById('fromLang').value != '' && document.getElementById('fromLang').value != 'not available')
		translate();
}

// use locally stored text and match with elements in API
function detect() 
{
	var localText = document.getElementById("srcTextArea").value;
	sendToAPI = "http://api.microsofttranslator.com/V2/Ajax.svc/Detect?oncomplete=onDetect&appId=86B37AB482F841F7A33D2AB16430EF7C46E8775F&text=" + localText;
	callAPI(sendToAPI);
}

// users can select languages using dropdown menu
function languageOptions() 
{
	selectbox = document.getElementById('toLanguageSelect');

	for(var i=0; i<allLanguageCodes.length; i++) 
	{
		var languageChoice = document.createElement("OPTION");

		languageChoice.text = allLanguageNames[i];
		languageChoice.value = allLanguageNames[i];

		selectbox.appendChild(languageChoice);

		// set default to browser language
		if(allLanguageCodes[i] == 'en')
			languageChoice.selected = true;

		codeToLanguage[allLanguageCodes[i]] = allLanguageNames[i];
		languageToCode[allLanguageNames[i]] = allLanguageCodes[i]
	}
}

function selectLanguage(message)
{
	allLanguageNames = message;
	languageOptions();
}

// begin conversion from code to name
function getLanguageNames(languageCodes)
{
	var name = 'en';
	var code = "[";
	for (var i = 0; i < languageCodes.length; i++) 
	{
		code += "\"" + languageCodes[i] + "\"";
		if (i != languageCodes.length - 1)
			code += ", ";
	}
	code += "]";
	
	sendToAPI = "http://api.microsofttranslator.com/V2/Ajax.svc/GetLanguageNames" 
	+ "?oncomplete=selectLanguage&appId=86B37AB482F841F7A33D2AB16430EF7C46E8775F"
	+ "&locale=" + name
	+ "&languageCodes=" + code;
	
	callAPI(sendToAPI);
}

function convert(message)
{
	allLanguageCodes = message;
	getLanguageNames(allLanguageCodes);
}

// API method to get updated list of languages
function getLanguagesForTranslate()
{
	api = "http://api.microsofttranslator.com/V2/Ajax.svc/GetLanguagesForTranslate?oncomplete=convert&appId=86B37AB482F841F7A33D2AB16430EF7C46E8775F";
	callAPI(api);
}

window.onload = getLanguagesForTranslate;