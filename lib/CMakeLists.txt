MACRO (FILE_TO_STRING infile outfile var)
ADD_CUSTOM_COMMAND(OUTPUT ${outfile}
                   COMMAND ${CMAKE_COMMAND}
                   ARGS -Dinfile:FILEPATH=xml/${infile}
                        -Doutfile:FILEPATH=${outfile}
                        -Dvar:STRING=${var}
                        -P filetostring.cmake
                   MAIN_DEPENDENCY
                        xml/${infile}
)
ENDMACRO (FILE_TO_STRING)

FILE_TO_STRING(form_templates.xml form_templates_xml.cpp form_templates_xml)
FILE_TO_STRING(messages.xml messages_xml.cpp messages_xml)
FILE_TO_STRING(country_names.xml country_names_xml.cpp country_names_xml)

SET (ojudge_lib
form_templates_xml.cpp
messages_xml.cpp
country_names_xml.cpp
datastore/CategoryStore.cpp
datastore/CountryStore.cpp
datastore/DataStore.cpp
datastore/LanguageStore.cpp
datastore/ProblemStore.cpp
datastore/UserStore.cpp
dbmodel/DBModel.cpp
external/QrCode.cpp
viewmodels/CategoryModel.cpp
viewmodels/CountryModel.cpp
viewmodels/LanguageModel.cpp
viewmodels/ProblemModel.cpp
viewmodels/UserModel.cpp
viewmodels/ViewModels.cpp
widgets/OJCodeEditorWidget.cpp
widgets/OJColorPicker.cpp
widgets/OJProblemViewerWidget.cpp
widgets/OJRatingSetWidget.cpp
widgets/OJRatingViewWidget.cpp
AboutWidget.cpp
AdminWidget.cpp
AuthWidget.cpp
ContactWidget.cpp
ContestsWidget.cpp
ContributeWidget.cpp
DashboardWidget.cpp
FactsWidget.cpp
FooterWidget.cpp
Joomla10HashFunction.cpp
HomeWidget.cpp
LanguagesWidget.cpp
LoginWidget.cpp
OJAuthModel.cpp
OJUtils.cpp
PdfResource.cpp
ProblemsWidget.cpp
ProblemWidget.cpp
ProfileWidget.cpp
RankingsWidget.cpp
RegistrationView.cpp
SponsorsWidget.cpp
SubmissionsWidget.cpp
TeamWidget.cpp
TutorialWidget.cpp
ojudgeApp.cpp
)

add_library (ojudge SHARED ${ojudge_lib})

set_target_properties (
        ojudge
        PROPERTIES
        SOVERSION ${OJUDGE_VERSION_MAJOR}
        VERSION ${OJUDGE_VERSION_MAJOR}.${OJUDGE_VERSION_MINOR}.${OJUDGE_VERSION_RELEASE}
)

include_directories(
	PkgConfig::GRAPHICSMAGICK
)

TARGET_LINK_LIBRARIES (ojudge
        ${Boost_LIBRARIES}
        Wt::Wt
        Wt::HTTP
        Wt::Dbo
        Wt::DboPostgres
        PkgConfig::OATH
        PkgConfig::GRAPHICSMAGICK
)
