cmake_minimum_required(VERSION 3.12)

include(CMakeToolsHelpers OPTIONAL) 

project(Skyscraper CXX)

set(CMAKE_AUTOMOC ON)
find_package(Qt5 COMPONENTS Core Gui Xml Network REQUIRED)

set(SOURCES
	"src/abstractfrontend.cpp"
	"src/abstractfrontend.h"
	"src/abstractscraper.cpp"
	"src/abstractscraper.h"
	"src/arcadedb.cpp"
	"src/arcadedb.h"
	"src/attractmode.cpp"
	"src/attractmode.h"
	"src/cache.cpp"
	"src/cache.h"
	"src/compositor.cpp"
	"src/compositor.h"
	"src/crc32.cpp"
	"src/crc32.h"
	"src/emulationstation.cpp"
	"src/emulationstation.h"
	"src/esgamelist.cpp"
	"src/esgamelist.h"
	"src/fxbalance.cpp"
	"src/fxbalance.h"
	"src/fxblur.cpp"
	"src/fxblur.h"
	"src/fxbrightness.cpp"
	"src/fxbrightness.h"
	"src/fxcolorize.cpp"
	"src/fxcolorize.h"
	"src/fxcontrast.cpp"
	"src/fxcontrast.h"
	"src/fxframe.cpp"
	"src/fxframe.h"
	"src/fxgamebox.cpp"
	"src/fxgamebox.h"
	"src/fxhue.cpp"
	"src/fxhue.h"
	"src/fxmask.cpp"
	"src/fxmask.h"
	"src/fxopacity.cpp"
	"src/fxopacity.h"
	"src/fxrotate.cpp"
	"src/fxrotate.h"
	"src/fxrounded.cpp"
	"src/fxrounded.h"
	"src/fxsaturation.cpp"
	"src/fxsaturation.h"
	"src/fxscanlines.cpp"
	"src/fxscanlines.h"
	"src/fxshadow.cpp"
	"src/fxshadow.h"
	"src/fxstroke.cpp"
	"src/fxstroke.h"
	"src/gameentry.cpp"
	"src/gameentry.h"
	"src/igdb.cpp"
	"src/igdb.h"
	"src/imgtools.cpp"
	"src/imgtools.h"
	"src/importscraper.cpp"
	"src/importscraper.h"
	"src/layer.cpp"
	"src/layer.h"
	"src/localscraper.cpp"
	"src/localscraper.h"
	"src/main.cpp"
	"src/mobygames.cpp"
	"src/mobygames.h"
	"src/nametools.cpp"
	"src/nametools.h"
	"src/netcomm.cpp"
	"src/netcomm.h"
	"src/openretro.cpp"
	"src/openretro.h"
	"src/platform.cpp"
	"src/platform.h"
	"src/queue.cpp"
	"src/queue.h"
	"src/scraperworker.cpp"
	"src/scraperworker.h"
	"src/screenscraper.cpp"
	"src/screenscraper.h"
	"src/scripter.cpp"
	"src/scripter.h"
	"src/settings.h"
	"src/skyscraper.cpp"
	"src/skyscraper.h"
	"src/strtools.cpp"
	"src/strtools.h"
	"src/thegamesdb.cpp"
	"src/thegamesdb.h"
	"src/worldofspectrum.cpp"
	"src/worldofspectrum.h"
	"src/xmlreader.cpp"
	"src/xmlreader.h"
)

set(RESOURCES
	"config.ini.example"
	"README.md"
	"hints.txt"
	"artwork.xml.example1"
	"artwork.xml.example2"
	"artwork.xml.example3"
	"artwork.xml.example4"
	"mameMap.csv"
	"tgdb_developers.json"
	"tgdb_publishers.json"
	"artwork.xml"
	"aliasMap.csv"
)
list(TRANSFORM RESOURCES PREPEND "${PROJECT_SOURCE_DIR}/")

set(RESOURCES_SUBDIR_CACHE
	"cache/priorities.xml.example"
)
list(TRANSFORM RESOURCES_SUBDIR_CACHE PREPEND "${PROJECT_SOURCE_DIR}/")

set(RESOURCES_SUBDIR_DOCS
	"docs/CACHE.md"
)
list(TRANSFORM RESOURCES_SUBDIR_DOCS PREPEND "${PROJECT_SOURCE_DIR}/")

set(RESOURCES_SUBDIR_IMPORT
	"docs/IMPORT.md"
	"import/definitions.dat.example1"
	"import/definitions.dat.example2"
)
list(TRANSFORM RESOURCES_SUBDIR_IMPORT PREPEND "${PROJECT_SOURCE_DIR}/")

set(RESOURCES_SUBDIR_RESOURCES
	"resources/boxfront.png"
	"resources/boxside.png"
	"resources/maskexample.png"
	"resources/frameexample.png"
	"resources/scanlines1.png"
	"resources/scanlines2.png"
)
list(TRANSFORM RESOURCES_SUBDIR_RESOURCES PREPEND "${PROJECT_SOURCE_DIR}/")


add_executable(Skyscraper ${SOURCES} ${PLATFORM_SOURCES})
file(READ "VERSION" VERSION_IN OFFSET 8)
string(STRIP "${VERSION_IN}" VERSION_IN)
target_compile_definitions(Skyscraper PRIVATE VERSION="${VERSION_IN}")
target_link_libraries(Skyscraper Qt5::Core Qt5::Gui Qt5::Xml Qt5::Network)

add_custom_command(
	TARGET Skyscraper POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E make_directory
		"$<TARGET_FILE_DIR:Skyscraper>/resources"
		"$<TARGET_FILE_DIR:Skyscraper>/import"
		"$<TARGET_FILE_DIR:Skyscraper>/cache"
		"$<TARGET_FILE_DIR:Skyscraper>/docs"
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        $<TARGET_FILE:Qt5::Core>
        $<TARGET_FILE:Qt5::Gui>
        $<TARGET_FILE:Qt5::Xml>
		$<TARGET_FILE:Qt5::Network>
		${RESOURCES}
		$<TARGET_FILE_DIR:Skyscraper>
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		${RESOURCES_SUBDIR_CACHE}
		"$<TARGET_FILE_DIR:Skyscraper>/cache"
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		${RESOURCES_SUBDIR_DOCS}
		"$<TARGET_FILE_DIR:Skyscraper>/docs"
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		${RESOURCES_SUBDIR_IMPORT}
		"$<TARGET_FILE_DIR:Skyscraper>/import"
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		${RESOURCES_SUBDIR_RESOURCES}
		"$<TARGET_FILE_DIR:Skyscraper>/resources"
)
