cmake_minimum_required(VERSION 2.8)

add_subdirectory(faceclustering)
add_subdirectory(facerecognition)

option(DOWNLOAD_DATA "Download dataset 'Labeled Faces in the Wild' [~173Mb] and uncompress it into data directory" OFF)

if(DOWNLOAD_DATA)

	# We are using the "Labeled Faces in the Wild" dataset (http://vis-www.cs.umass.edu/lfw/)
    if(EXISTS "${CMAKE_BINARY_DIR}/data/lfw.csv")
		message(STATUS "Data is already on disk (delete file 'lfw.csv' to force download)")
    else()
		message(STATUS "Downloading 'Labeled Faces in the Wild' dataset...")
		file(DOWNLOAD http://vis-www.cs.umass.edu/lfw/lfw.tgz ${CMAKE_BINARY_DIR}/data/lfw.tgz
			SHOW_PROGRESS
			EXPECTED_MD5 ac79dc88658530a91423ebbba2b07bf3)

		message(STATUS "Uncompressing dataset...")
		execute_process(COMMAND ${CMAKE_COMMAND} -E tar xvzf ${CMAKE_BINARY_DIR}/data/lfw.tgz
			WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/data)

		message(STATUS "Building lfw.csv file")
		execute_process(COMMAND python "${CMAKE_SOURCE_DIR}/utils/build_csv.py" "${CMAKE_BINARY_DIR}/data/lfw")

    endif()

endif()
