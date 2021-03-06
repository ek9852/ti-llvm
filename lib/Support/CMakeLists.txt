set(system_libs)
if( NOT MSVC )
  if( MINGW )
    set(system_libs ${system_libs} imagehlp psapi shell32)
  elseif( CMAKE_HOST_UNIX )
    if( HAVE_LIBRT )
      set(system_libs ${system_libs} rt)
    endif()
    if( HAVE_LIBDL )
      set(system_libs ${system_libs} ${CMAKE_DL_LIBS})
    endif()
    if(LLVM_ENABLE_TERMINFO)
      if(HAVE_TERMINFO)
        set(system_libs ${system_libs} ${TERMINFO_LIBS})
      endif()
    endif()
    if( LLVM_ENABLE_THREADS AND HAVE_LIBATOMIC )
      set(system_libs ${system_libs} atomic)
    endif()
    if( LLVM_ENABLE_THREADS AND HAVE_LIBPTHREAD )
      set(system_libs ${system_libs} pthread)
    endif()
    if ( LLVM_ENABLE_ZLIB AND HAVE_LIBZ )
      set(system_libs ${system_libs} z)
    endif()
    set(system_libs ${system_libs} m)
  endif( MINGW )
endif( NOT MSVC )

if( ${BUILD_OS} MATCHES "SYS_BIOS" )
set(SYS_BIOS_SRC_FILES "Bios/FileSystem.cpp")
endif( ${BUILD_OS} MATCHES "SYS_BIOS" )

add_llvm_library(LLVMSupport
  APFloat.cpp
  APInt.cpp
  APSInt.cpp
  ARMBuildAttrs.cpp
  ARMWinEH.cpp
  Allocator.cpp
  BlockFrequency.cpp
  BranchProbability.cpp
  circular_raw_ostream.cpp
  CommandLine.cpp
  Compression.cpp
  ConvertUTF.c
  ConvertUTFWrapper.cpp
  CrashRecoveryContext.cpp
  DataExtractor.cpp
  DataStream.cpp
  Debug.cpp
  DeltaAlgorithm.cpp
  DAGDeltaAlgorithm.cpp
  Dwarf.cpp
  ErrorHandling.cpp
  FileUtilities.cpp
  FileOutputBuffer.cpp
  FoldingSet.cpp
  FormattedStream.cpp
  GraphWriter.cpp
  Hashing.cpp
  IntEqClasses.cpp
  IntervalMap.cpp
  IntrusiveRefCntPtr.cpp
  IsInf.cpp
  IsNAN.cpp
  LEB128.cpp
  LineIterator.cpp
  Locale.cpp
  LockFileManager.cpp
  ManagedStatic.cpp
  MathExtras.cpp
  MemoryBuffer.cpp
  MemoryObject.cpp
  MD5.cpp
  Options.cpp
  PluginLoader.cpp
  PrettyStackTrace.cpp
  RandomNumberGenerator.cpp
  Regex.cpp
  ScaledNumber.cpp
  SmallPtrSet.cpp
  SmallVector.cpp
  SourceMgr.cpp
  SpecialCaseList.cpp
  Statistic.cpp
  StreamingMemoryObject.cpp
  StringExtras.cpp
  StringMap.cpp
  StringPool.cpp
  StringRef.cpp
  SystemUtils.cpp
  Timer.cpp
  ToolOutputFile.cpp
  Triple.cpp
  Twine.cpp
  Unicode.cpp
  YAMLParser.cpp
  YAMLTraits.cpp
  raw_os_ostream.cpp
  raw_ostream.cpp
  regcomp.c
  regerror.c
  regexec.c
  regfree.c
  regstrlcpy.c

# System
  Atomic.cpp
  DynamicLibrary.cpp
  Errno.cpp
  Host.cpp
  Memory.cpp
  Mutex.cpp
  Path.cpp
  Process.cpp
  Program.cpp
  RWMutex.cpp
  SearchForAddressOfSpecialSymbol.cpp
  Signals.cpp
  TargetRegistry.cpp
  ThreadLocal.cpp
  Threading.cpp
  TimeValue.cpp
  Valgrind.cpp
  Watchdog.cpp
  ${SYS_BIOS_SRC_FILES}

  ADDITIONAL_HEADERS
  Unix/Host.inc
  Unix/Memory.inc
  Unix/Mutex.inc
  Unix/Path.inc
  Unix/Process.inc
  Unix/Program.inc
  Unix/RWMutex.inc
  Unix/Signals.inc
  Unix/ThreadLocal.inc
  Unix/TimeValue.inc
  Unix/Watchdog.inc
  Windows/DynamicLibrary.inc
  Windows/Host.inc
  Windows/Memory.inc
  Windows/Mutex.inc
  Windows/Path.inc
  Windows/Process.inc
  Windows/Program.inc
  Windows/RWMutex.inc
  Windows/Signals.inc
  Windows/ThreadLocal.inc
  Windows/TimeValue.inc
  Windows/Watchdog.inc

  LINK_LIBS ${system_libs}
  )

set_property(TARGET LLVMSupport PROPERTY LLVM_SYSTEM_LIBS "${system_libs}")
