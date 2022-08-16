# SF_BuilderCustomMenuPrincipalCode

<script type='text/javascript' src='https://code.jquery.com/jquery-3.6.0.min.js'></script>
<script>
        
    (function(){    
                
        $(function(){   
            
            var modifs = [
                
                // 3.4 Déclarer le menu principal de l’application 
                {
                    selector: '.themeProfileMenu .menu-profile',
                    test: function(el) {
                        return el.parent().is('div');
                    },
                    action: function(el) {
                        el.parent().replaceWith('<div>'+ el.parent().html() +'</div>');
                    }
                },
                
                // 3.12 Optimiser l’accessibilité du menu principal de la version mobile
                {
                    selector: '.comm-navigation__mobile-trigger',
                    test: function(el) {
                        return el.parent().is('span');                        
                    },
                    action: function(el) {
                      el.replaceWith('<span class="themeNavTrigger comm-navigation__mobile-trigger" style="display: block;" data-aura-rendered-by="461:0">' + el.html() + '</span>');
                    }
                },
                
                // Title sur les liens
                {
                    selector: '.viewAllLink a',
                    test: function(el) {
                        return el.parent().is('td');
                    },
                    action: function(el) {                        
                        var title = el.attr('aria-label');
                        el.attr('title', title);
                    }
                },  
                
                {
                    selector: 'a',
                    test: function(el) {
                        
                    },
                    action: function(els) {
                       els.each(
                        	function(){
                                if ($(this).attr('target') != '') {
                                    var target = $(this).attr('target')
                                    var el = $(this);
                                    if (target === "_blank") {
                                        var title = $(this).text();
                                        $(el, this).attr('title', title + ' - Nouvelle fenêtre');
                                        
                                    }
                                }
                            }
                        );               
                    }
                },
                
                // 3.1 Déclarer la bannière de l’application
				{
					selector : '.header',
					test : function(el){
						return el.parent().is('header');
					},
					action: function(el){
						el.wrap('<header role="banner"></header>');                        
					}
				},
                          
                // 3.2 Déclarer le moteur de recherche de l’application
				{
					selector : '.themeUtil.themeSearch',
					test : function(el){
						return el.is('[role=search]');
					},
					action: function(el){
						el.attr({role:'search'});
					}
				},
                          
                // 3.3 Déclarer le menu profil de l’application
				{
					selector : '.comm-user-profile-menu',
					test : function(el){
						return el.parent().is('nav');
					},
					action: function(el){
						el.wrap('<nav role="navigation" aria-label="Menu profil"></nav>');
					}
				},
                
                // 3.4 Déclarer le menu principal de l’application
				{
					selector : 'nav.themeNavContainer',
					test : function(el){
						return el.is('[role=navigation]');
					},
					action: function(el){
						el.attr({role:'navigation'}).attr('aria-label', 'Menu principal');
					}
				},
                
                // 3.5 Déclarer la zone de contenu principal de l’application
                {
					selector : 'div.body',
					test : function(el){
						return el.parent().is('main');
					},
					action: function(el){
                        var elWidth = el.outerWidth();
                        el.wrap('<main role="main"></main>').parent().css({width:elWidth, margin:'0 auto'});
					}
				},
                
                // 3.6 Déclarer la zone de pied de page de l’application
                {
					selector : 'div.footer',
					test : function(el){
						return el.parent().is('footer');
					},
					action: function(el){
						el.wrap('<footer role="contentinfo" id="footer"></footer>');
					}
				},
                
                // 3.7 Déclarer des titres d’écrans explicites  
                /*{
					selector : 'head',
					/*test : function(el){
						return document.title.includes("Espace Professionnel Agefiph");
					},
					action: function(el){
						if (document.title.includes("Create Record")) {
                            document.title = 'Dépôt de demande | Espace Professionnel Agefiph';
                        }
                        else {
                            document.title = document.title + '| Espace Professionnel Agefiph';
                        }                     
                        
					}
				},*/
                
                // 3.9 Intégrer le logo de l’application sous forme d’image dans le code source
               /* {
					selector : 'div.logoImage',
					test : function(el){
						return el.children().eq(0).is('img');
					},
					action: function(el){
                        var logoBackgroundImage = el.css('backgroundImage');
                        logoBackgroundImage = logoBackgroundImage.substring(logoBackgroundImage.indexOf('"')+1);
                        logoBackgroundImage = logoBackgroundImage.substring(0, logoBackgroundImage.indexOf('"'));
						var logo = $('<img/>').attr({src: logoBackgroundImage, role: 'img', 'arial-label': "Agefiph (retour à l’accueil)"}).css('max-height', '100%').appendTo(el);
                        el.css('text-align', 'center');
					}
				},*/
                
                {
					selector : 'div.logoImage',
					test : function(el){
						return el.children().eq(0).is('span');
					},
					action: function(el){
                        var logoBackgroundImage = el.css('backgroundImage');
                        logoBackgroundImage = logoBackgroundImage.substring(logoBackgroundImage.indexOf('"')+1);
                        logoBackgroundImage = logoBackgroundImage.substring(0, logoBackgroundImage.indexOf('"'));
						var logo = $('<img/>').attr({src: logoBackgroundImage, alt: 'Agefiph - Ouvrir l’emploi aux personnes handicapées (retour à l’accueil de l’espace personnel)'}).css('max-height', '100%').appendTo(el);
                        logo.wrap('<span class="logo-agefiph"></span>');
                        $(".logo-agefiph").css('width', '100%');
                        el.css('text-align', 'center');
					}
				},
                
                // 3.9 Intégrer le logo de l’application sous forme d’image dans le code source
                {
					selector : 'a.forceCommunityThemeLogo',
               		test : function(el){
						return el.attr('href') == '/EspacePersonnel';
					},
					action: function(el){
						el.attr({href:'/EspacePersonnel'});
					}
				},
                
                // 3.10 Associer un titre à chaque tableau de données
                {
					selector : function(){
                        return $('div.forceCommunityRecordListStandard').filter(
                            function(){
                                return $('caption', this).length == 0;
                            }
                        );
                    },
					action: function(els){
                        els.each(
                        	function(){
                                var el = $(this);
                                var tbl = el.find('table');
                                var ttl = el.find('h2.listTitle').text();
                                if(ttl){
                                	$('<caption/>').text(ttl).attr('class', 'assistiveText').prependTo(tbl);
                                }
                            }
                        );
					}
				},
                
                // 3.10 Associer un titre à chaque tableau de données
                {
					selector : function(){
                        return $('div.forceListViewManager').filter(
                            function(){
                                return $('caption', this).length == 0;
                            }
                        );
                    },
					action: function(els){
                        els.each(
                        	function(){
                                var el = $(this);
                                var tbl = el.find('table');
                                var ttl = el.find('span.forceBreadCrumbItem').text();
                                if(ttl){
                                	$('<caption/>').text(ttl).attr('class', 'assistiveText').prependTo(tbl);
                                }
                            }
                        );
					}
				},
                
                // 3.10 Associer un titre à chaque tableau de données
                {
					selector : function(){
                        return $('div.forceRelatedListSingleContainer').filter(
                            function(){
                                return $('caption', this).length == 0;
                            }
                        );
                    },
					action: function(els){
                        els.each(
                        	function(){
                                var el = $(this);
                                var tbl = el.find('table');
                                var ttl = el.find('h2.slds-card__header-title').text();
                                if(ttl){
                                	$('<caption/>').text(ttl).attr('class', 'assistiveText').prependTo(tbl);
                                }
                            }
                        );
					}
				},
                
                // 3.11 Déclarer les tableaux de mise en forme  
                {
					selector : 'table.footerinfo',
               		test : function(el){
						return el.is('[role=presentation]');
					},
					action: function(el){
						el.attr({role:'presentation'});                        
					}
				},
                
                // 3.12 Optimiser l’accessibilité du menu principal de la version mobile 
				{
					selector : 'button.forceCommunityThemeNavTrigger',
					test : function(el){
						return el.parent().is('nav');
					},
					action: function(el){
						el.wrap('<nav role="navigation" aria-label="Menu principal"></nav>');
					}
				},   
                
                // 3.13
               /* {
					selector : '.comm-user-profile-menu',
               		test : function(el){						
                      
					},
					action: function(el){						
                      // el.wrap('<a href="/EspacePersonnel/secur/logout.jsp?" >Déconnexion</a>');
                       el.wrap('<ul class="menu-profile"><li><a href="/EspacePersonnel/s/comm-my-account">Mon compte</a></li><li><a href="/EspacePersonnel/secur/logout.jsp?">Se déconnecter</a></li></ul>');
                       el.remove();                        
					}
				},*/
                
               /* {
					selector : function(){
                        return $('div.subMenu').filter(
                            function(){
                                return $('div', this).length > 0;
                            }
                        )},
					action: function(els){
                        
                        els[O].attr({id:'menu-plus'}); 
                    
					}
					
				},*/
                
                {
					selector : function(){
                        return $('div.subMenu').filter(
                            function(){
                                return $('div', this).length > 0;
                            }
                        );
                    },
					action: function(els){
                        els.each(
                        	function(){
                                var el = $(this);
								els.attr({id:'menu-plus'});
                            }
                        );
					}
				},
                
                {
					selector : '.comm-navigation__top-level-item-link',
               		test : function(el){						
                      return el.is('[aria-controls=menu-plus]');
					},
					action: function(el){						
						el.attr('aria-controls', 'menu-plus');                       
					}
				}//,
                // Suppression des éléments 
          /*      {
					selector : 'head',
					test : function(el){
						
					},
					action: function(el){						
                        document.querySelector('[title="Ajouter des fichiers"]').remove();
                        document.querySelector('[title="Épingler cette vue de liste"]').remove();
                        document.querySelector('[title="Modifier la liste"]').remove();
                        document.querySelector('[title="Afficher des graphiques"]').remove();
                        document.querySelector('[title="Afficher les filtres"]').remove();
                        document.querySelector('[id="listviewSearchTooltip-13]').remove();
                        document.querySelector('[title="Actualiser"]').remove();
                        document.querySelector('[placeholder="Recherchez dans cette liste..."]').remove();
                        document.querySelector('[title="Modifier"]').remove();
                        document.querySelector('[title="Nouveau"]').remove();
                        document.querySelector('[title="Afficher les filtres rapides"]').remove();
                        $( ".inline-edit-trigger" ).remove();
                        $( ".countSortedByFilteredBy" ).remove();
                        $( ".slds-input__icon" ).remove();
                        $( ".slds-button--icon-border-filled" ).remove();
                        $( ".slds-button_icon-more" ).remove();
                        $( ".forceListViewPicker" ).remove();
                        $( ".slds-file-selector__body" ).remove();
                        $( ".userProfileTabs " ).remove();
                        $( ".slds-border_right" ).remove();                                 
					}
				}*/
			];
			
			var intVal = 500;
						
			setInterval(
				function(){
                    
					for(var i = 0; i < modifs.length ; i++){
                        
						var modif = modifs[i];
												                        
                        if($.isFunction(modif.selector)){
                           
							var els = modif.selector();
                            
                            if(els.length > 0){
                                modif.action(els);
                            }
                        }
                        else{
                        	var el = $(modif.selector);    
                            
                            if(el.length > 0 && !modif.test(el)){
								modif.action(el);
							}
                        }
					
					}
				},
				intVal
			);
        });
    })();
    
	// Reload the page when screen size changes (mobile <> desktop)
    // Some components doesn't refresh the layout when user change between mobile / desktop
    var firstRun = true;
    var beforeResizeWidth = window.screen.width;
    var afterResizeWidth;
    var mobileBreakpoint = 600;
    function reportWindowSize() {
        if ((firstRun && beforeResizeWidth > mobileBreakpoint && afterResizeWidth <= mobileBreakpoint) || (firstRun && beforeResizeWidth <= mobileBreakpoint && afterResizeWidth > mobileBreakpoint)){
        	firstRun = false;
            location.reload();
        }        
    }
	window.onresize = function(){
        afterResizeWidth = window.screen.width;
        reportWindowSize()
    };
</script>
