<html>
        <style type='text/css'>
        	.embeddedServiceHelpButton .helpButton .uiButton {
        		background-color: #005290;
        		font-family: "Arial", sans-serif;
        	}
        	.embeddedServiceHelpButton .helpButton .uiButton:focus {
        		outline: 1px solid #005290;
        	}
        </style>
        
        <script type='text/javascript' src='https://service.force.com/embeddedservice/5.0/esw.min.js'></script>
        <script type='text/javascript'>
        	var initESW = function(gslbBaseURL) {
        		embedded_svc.settings.displayHelpButton = true; //Or false
        		embedded_svc.settings.language = ''; //For example, enter 'en' or 'en-US'

        		embedded_svc.settings.prepopulatedPrechatFields = {
        		    FirstName: "Test",
        		    LastName: "Test",
        		    Email: "test@test.com",
        		    Subject: "Test Subject"
        		};

                        embedded_svc.settings.extraPrechatFormDetails = [
                                {
                                        "label":"Text", 
                                        "transcriptFields": [ "Text__c" ] 
                                }, 
                                {
                                        "label":"Skill", 
                                        "transcriptFields": [ "Skill__c" ] 
                                }, 
                                {
                                        "label":"Transcript", 
                                        "transcriptFields": [ "Transcript__c" ] 
                                }
                        ];
        
        		embedded_svc.settings.enabledFeatures = ['LiveAgent'];
        		embedded_svc.settings.entryFeature = 'LiveAgent';
        
        		embedded_svc.init(
        			'https://infallibletechie9-dev-ed.my.salesforce.com',
        			'https://infallibletechie9-dev-ed.my.site.com/',
        			gslbBaseURL,
        			'00D5f000001yZYJ',
        			'Embedded_Chat',
        			{
        				baseLiveAgentContentURL: 'https://c.la4-c1-ia5.salesforceliveagent.com/content',
        				deploymentId: '5725f000000TllG',
        				buttonId: '5735f000000Tm6g',
        				baseLiveAgentURL: 'https://d.la4-c1-ia5.salesforceliveagent.com/chat',
        				eswLiveAgentDevName: 'EmbeddedServiceLiveAgent_Parent04I5f000000PJ1GEAW_17c32b6b665',
        				isOfflineSupportEnabled: true
        			}
        		);
        	};
        
        	if (!window.embedded_svc) {
        		var s = document.createElement('script');
        		s.setAttribute('src', 'https://infallibletechie9-dev-ed.my.salesforce.com/embeddedservice/5.0/esw.min.js');
        		s.onload = function() {
        			initESW(null);
        		};
        		document.body.appendChild(s);
        	} else {
        		initESW('https://service.force.com');
        	}
         
                document.addEventListener(
                        "setCustomField",
                        function( event ) {
                                console.log(
                                        'Inside the Custom Field Listener'
                                );
                                console.log(
                                        'Passed Text value is',
                                        event.detail.text
                                );
                                console.log(
                                        'Passed Skill value is',
                                        event.detail.skill
                                );
                                embedded_svc.settings.extraPrechatFormDetails[ 0 ].value = event.detail.text;
                                embedded_svc.settings.extraPrechatFormDetails[ 1 ].value = event.detail.skill;
                                if (event.detail.transcript && event.detail.transcript.length > 131072) {
                                    embedded_svc.settings.extraPrechatFormDetails[ 2 ].value = event.detail.transcript.slice(0, 131070);
                                } else {
                                    embedded_svc.settings.extraPrechatFormDetails[ 2 ].value = event.detail.transcript;
                                }
                                event.detail.callback();
                        },
                        false
                );
        </script>

        <script>
                document.addEventListener( 'keyup', ( event ) => {
                        console.log(
                                'Key is',
                                event.key
                        );
                        if ( event.key == 115 ) {
                                console.log(
                                        "S key was pressed"
                                );
                                embedded_svc.liveAgentAPI.startChat({
                                        directToAgentRouting: {
                                                buttonId: "5735f000000Tm6g",
                                                fallback: true
                                        },
                                        extraPrechatInfo: [],
                                        extraPrechatFormDetails: []
                                });
                        }
                };
        </script>
</html>
