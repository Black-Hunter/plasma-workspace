########### next target ###############
#set(EXTERNAL_LIBS KIOCore Qt5::Script KF5::Plasma Qt5::Sql)

add_definitions(-DTRANSLATION_DOMAIN=\"plasma_runner_bookmarksrunner\")

find_package(Qt5 CONFIG REQUIRED COMPONENTS Sql)


set(EXTERNAL_LIBS
    Qt5::Sql
    KF5::KIOCore
    KF5::I18n
    KF5::Runner
    KF5::Bookmarks
)

set(krunner_bookmarks_common_SRCS
    bookmarkmatch.cpp
    faviconfromblob.cpp
    favicon.cpp
    fetchsqlite.cpp
)

ecm_qt_declare_logging_category(krunner_bookmarks_common_SRCS
    HEADER bookmarks_debug.h
    IDENTIFIER RUNNER_BOOKMARKS
    CATEGORY_NAME org.kde.plasma.runner.bookmarks
    DEFAULT_SEVERITY Warning
)

set(krunner_bookmarksrunner_SRCS
    ${krunner_bookmarks_common_SRCS}
    bookmarksrunner.cpp
    browserfactory.cpp
    browsers/chromefindprofile.cpp
    browsers/chrome.cpp
    browsers/firefox.cpp
    browsers/kdebrowser.cpp
    browsers/opera.cpp
)


add_library(krunner_bookmarksrunner MODULE ${krunner_bookmarksrunner_SRCS})
target_link_libraries(krunner_bookmarksrunner ${EXTERNAL_LIBS})

set(krunner_bookmarks_test_SRCS
    ${krunner_bookmarks_common_SRCS}
    browsers/chromefindprofile.cpp
    browsers/chrome.cpp
 )

add_library(krunner_bookmarks_test STATIC ${krunner_bookmarks_test_SRCS})
target_link_libraries(krunner_bookmarks_test
    Qt5::Sql
    KF5::KIOCore
    KF5::Runner
)

install(TARGETS krunner_bookmarksrunner DESTINATION ${KDE_INSTALL_PLUGINDIR} )


########### install files ###############

install(FILES plasma-runner-bookmarks.desktop DESTINATION ${KDE_INSTALL_KSERVICES5DIR})

# Currently tests include only chrome, so no need to get include them if json is not found
if(BUILD_TESTING)
   add_subdirectory(tests)
endif()
