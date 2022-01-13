#[[
A library holding image and color data for the four built-in themes
]]

# Names of the image files (and Colors.txt) expected in each theme
set( IMAGE_LIST
   AudacityLogo48x48.png
   BandWidthCursor.png
   BottomFrequencyCursor.png
   Colors.txt
   Copy.png
   CopyDisabled.png
   CrosshairCursor.png
   Cut.png
   CutDisabled.png
   CutPreview.png
   CutPreviewDisabled.png
   DisabledCursor.png
   DownButtonExpand.png
   DownButtonExpandSel.png
   DownButtonLarge.png
   DownButtonSmall.png
   Draw.png
   DrawCursor.png
   EditEffects.png
   EnvCursor.png
   Envelope.png
   FFwd.png
   FFwdDisabled.png
   GrabberDropLoc.png
   HelpIcon.png
   HiliteButtonExpand.png
   HiliteButtonExpandSel.png
   HiliteButtonLarge.png
   HiliteButtonSmall.png
   HiliteUpButtonExpand.png
   HiliteUpButtonExpandSel.png
   HiliteUpButtonLarge.png
   HiliteUpButtonSmall.png
   IBeam.png
   IBeamCursor.png
   LabelCursorLeft.png
   LabelCursorRight.png
   LabelGlyph0.png
   LabelGlyph1.png
   LabelGlyph10.png
   LabelGlyph11.png
   LabelGlyph2.png
   LabelGlyph3.png
   LabelGlyph4.png
   LabelGlyph5.png
   LabelGlyph6.png
   LabelGlyph7.png
   LabelGlyph8.png
   LabelGlyph9.png
   Loop.png
   LoopDisabled.png
   MacDownButton.png
   MacDownButtonSmall.png
   MacHiliteButton.png
   MacHiliteButtonSmall.png
   MacHiliteUpButton.png
   MacHiliteUpButtonSmall.png
   MacUpButton.png
   MacUpButtonSmall.png
   Mic.png
   Multi.png
   Options.png
   OptionsDisabled.png
   Paste.png
   PasteDisabled.png
   Pause.png
   PauseDisabled.png
   Play.png
   PlayDisabled.png
   PlayPointer.png
   PlayPointerPinned.png
   Record.png
   RecordBelow.png
   RecordBelowDisabled.png
   RecordBeside.png
   RecordBesideDisabled.png
   RecordDisabled.png
   RecordPointer.png
   RecordPointerPinned.png
   Redo.png
   RedoDisabled.png
   Rewind.png
   RewindDisabled.png
   Scrub.png
   ScrubDisabled.png
   Seek.png
   SeekDisabled.png
   Silence.png
   SilenceDisabled.png
   SliderThumb.png
   SliderThumbHilited.png
   SliderThumbRotated.png
   SliderThumbRotatedHilited.png
   Speaker.png
   SpectralBrush.png
   Stop.png
   StopDisabled.png
   SubViewsCursor.png
   SyncLockIcon.png
   SyncLockSelTile.png
   SyncLockTracksDisabled.png
   SyncLockTracksDown.png
   SyncLockTracksUp.png
   TimeCursor.png
   TnAutomateSelection.png
   TnAutomateSelectionDisabled.png
   TnCalibrate.png
   TnCalibrateDisabled.png
   TnEndOff.png
   TnEndOffDisabled.png
   TnEndOn.png
   TnEndOnDisabled.png
   TnMakeTag.png
   TnMakeTagDisabled.png
   TnSelectSilence.png
   TnSelectSilenceDisabled.png
   TnSelectSound.png
   TnSelectSoundDisabled.png
   TnStartOff.png
   TnStartOffDisabled.png
   TnStartOn.png
   TnStartOnDisabled.png
   ToggleScrubRuler.png
   TopFrequencyCursor.png
   Trim.png
   TrimDisabled.png
   Undo.png
   UndoDisabled.png
   UpButtonExpand.png
   UpButtonExpandSel.png
   UpButtonLarge.png
   UpButtonSmall.png
   Zoom.png
   ZoomFit.png
   ZoomFitDisabled.png
   ZoomIn.png
   ZoomInCursor.png
   ZoomInDisabled.png
   ZoomOut.png
   ZoomOutCursor.png
   ZoomOutDisabled.png
   ZoomSel.png
   ZoomSelDisabled.png
   ZoomToggle.png
   ZoomToggleDisabled.png
)

# One custom command for each theme, invoking the image compiler if the
# generated header file is out of date with its image and color definitions
foreach( pair
   "classic;ClassicThemeAsCeeCode"
   "dark;DarkThemeAsCeeCode"
   "high-contrast;HighContrastThemeAsCeeCode"
   "light;LightThemeAsCeeCode"
)
   list( POP_FRONT pair theme filename )
   set( THEME_FILES )
   list( TRANSFORM IMAGE_LIST PREPEND "${theme}/Components/"
      OUTPUT_VARIABLE THEME_FILES )
   set( arguments ${CMAKE_CURRENT_SOURCE_DIR} ${theme} )

   if( CMAKE_SYSTEM_NAME MATCHES "Darwin" )
      # Need a shell invocation that sets DYLD_LIBRARY_PATH environment variable
      string( JOIN " " argument_string
         DYLD_LIBRARY_PATH=${_SHARED_PROXY_BASE_PATH}/$<CONFIG>/
         $<TARGET_FILE:image-compiler>
         ${arguments} )
      set( command sh -v -c "${argument_string}" )
   else ()
      set( command image-compiler ${arguments} )
   endif ()

   add_custom_command( OUTPUT "${CMAKE_CURRENT_SOURCE_DIR}/${filename}.h"
      COMMAND ${command}
      DEPENDS ${THEME_FILES}
      COMMENT "Generating ${filename}.h..."
   )
endforeach()

# Now define a more conventional library, using the output of the image compiler
set( SOURCES
   ClassicThemeAsCeeCode.cpp
   ClassicThemeAsCeeCode.h
   DarkThemeAsCeeCode.cpp
   DarkThemeAsCeeCode.h
   HighContrastThemeAsCeeCode.cpp
   HighContrastThemeAsCeeCode.h
   LightThemeAsCeeCode.cpp
   LightThemeAsCeeCode.h
   LoadThemeResources.h
   LoadThemeResources.cpp
)
set( LIBRARIES
   lib-theme-interface
   PRIVATE
   wxwidgets::wxwidgets
 )
audacity_library( lib-theme-resources "${SOURCES}" "${LIBRARIES}"
   "" ""
)