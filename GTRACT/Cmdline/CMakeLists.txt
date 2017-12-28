
FindITKUtil( DWIConvert_ITK
        ITKImageIntensity
        ITKImageStatistics
        ITKTestKernel
        ITKIODCMTK
        ITKIORAW
        ITKDCMTK
        ITKNrrdIO
        )

FindVTKUtil(
  vtkIOLegacy
  vtkIOXML
  vtkCommonDataModel
  vtkFiltersCore
  vtkFiltersGeneral
)

set(DWIConvertDir /Users/hxie1/Projects/BrainsTools/DWIConvert)
include_directories(${DWIConvertDir})
set(DWIConvertSrc
        ${DWIConvertDir}/NRRDDWIConverter.h
        ${DWIConvertDir}/NRRDDWIConverter.cxx
        ${DWIConvertDir}/FSLDWIConverter.h
        ${DWIConvertDir}/FSLDWIConverter.cxx
        ${DWIConvertDir}/DWIConvertUtils.h
        ${DWIConvertDir}/DWIConvertUtils.cxx
        ${DWIConvertDir}/DWIConverterFactory.h
        ${DWIConvertDir}/DWIConverterFactory.cxx
        ${DWIConvertDir}/DWIConverter.h
        ${DWIConvertDir}/DWIConverter.cxx
        ${DWIConvertDir}/DWIDICOMConverterBase.h
        ${DWIConvertDir}/DWIDICOMConverterBase.cxx
        ${DWIConvertDir}/GEDWIConverter.h
        ${DWIConvertDir}/GenericDWIConverter.h
        ${DWIConvertDir}/HitachiDWIConverter.h
        ${DWIConvertDir}/PhilipsDWIConverter.h
        ${DWIConvertDir}/SiemensDWIConverter.h
        ${DWIConvertDir}/StringContains.h
        ${DWIConvertDir}/GEDWIConverter.cxx
        ${DWIConvertDir}/GenericDWIConverter.cxx
        ${DWIConvertDir}/HitachiDWIConverter.cxx
        ${DWIConvertDir}/PhilipsDWIConverter.cxx
        ${DWIConvertDir}/SiemensDWIConverter.cxx
        ${DWIConvertDir}/SiemensDWIConverter.hxx
        ${DWIConvertDir}/DWIConvertLib.cxx
        ${DWIConvertDir}/DWIConvertLib.h
        )


option(DEBUGGING_PRINT_IMAGES "Use the DEBUG_IMAGE_VIEWER for debugging" ON)
### A local macro for configuring all these programs consistantly for the different platforms.
###
set(ALL_PROGS_LIST
  gtractImageConformity
  gtractInvertRigidTransform
  gtractTransformToDisplacementField
  gtractInvertBSplineTransform
  gtractResampleCodeImage
  gtractResampleFibers
  compareTractInclusion
  gtractAnisotropyMap
  gtractTensor
  gtractCreateGuideFiber
  gtractFiberTracking
  gtractResampleAnisotropy
  gtractResampleB0
  extractNrrdVectorIndex
  gtractAverageBvalues
#  gtractConcatDwi
  gtractCostFastMarching
  gtractFastMarchingTracking
  gtractCopyImageOrientation
  gtractClipAnisotropy
  gtractInvertDisplacementField
  gtractResampleDWIInPlace
  gtractCoregBvalues
  gtractCoRegAnatomy
#  gtractCoRegAnatomyRigid
#  gtractCoRegAnatomyBspline
#  gtractFreeTracking
#  gtractStreamlineTracking
#  gtractGuidedTracking
#  gtractGraphSearchTracking
  )
foreach(prog ${ALL_PROGS_LIST})
  StandardBRAINSBuildMacro(NAME ${prog} TARGET_LIBRARIES GTRACTCommon BRAINSCommonLib ${VTK_LIBRARIES})
endforeach()

if(BUILD_TESTING AND NOT BRAINSTools_DISABLE_TESTING)
   add_subdirectory(TestSuite)
endif()

set(CancatDWI_SOURCE_FILES
        gtractConcatDwi.cxx
        ${DWIConvertSrc}
        )


add_executable(gtractConcatDwi ${CancatDWI_SOURCE_FILES})
target_link_libraries(gtractConcatDwi ${DWIConvert_ITK_LIBRARIES} BRAINSCommonLib ${DWIConvertSupportLib})