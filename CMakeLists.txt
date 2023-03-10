# Add target to install translations
find_package(Gettext)

if(GETTEXT_FOUND)
	# Discover our translations
	file(READ LINGUAS linguas)
	string(REPLACE " " ";" linguas ${linguas})
	set(linguas ${linguas})
	# Copy translations to build tree to avoid modifying source
	foreach(lang ${linguas})
		string(STRIP ${lang} lang)
		set(po ${lang}.po)
		file(COPY ${po} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
		list(APPEND pos ${CMAKE_CURRENT_BINARY_DIR}/${po})
	endforeach()
	GETTEXT_CREATE_TRANSLATIONS(popt.pot ALL ${pos})
	foreach(lang ${languages})
		install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${lang}.gmo
			DESTINATION ${CMAKE_INSTALL_LOCALEDIR}/${lang}/LC_MESSAGES/
			RENAME popt.mo
		)
	endforeach()

endif()
