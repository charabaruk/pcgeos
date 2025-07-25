## 3.2 Structures D-F

----------
#### DACPlayFlags
    DACPlayFlags        record
        DACPF_CATENATE          :1
                                :7
    DACPlayFlags        end

**Library:** sound.def

----------
#### DACReferenceByte
    DACReferenceByte        etype       word, 0, 1
        DACRB_NO_REFERENCE_BYTE     enum        DACReferenceByte
        DACRB_WITH_REFERENCE_BYTE   enum        DACReferenceByte

**Library:** sound.def

----------
#### DACSampleFormat
    DACSampleFormat     etype       word, 0, 1
        DACSF_8_BIT_PCM         enum        DACSampleFormat
        DACSF_2_TO_1_ADPCM      enum        DACSampleFormat
        DACSF_3_TO_1_ADPCM      enum        DACSampleFormat
        DACSF_4_TO_1_ADPCM      enum        DACSampleFormat

**Library:** sound.def

----------
#### DateTimeFormat
    DateTimeFormat      etype       word
        DTF_LONG                    enum    DateTimeFormat ; Sunday, March 5th, 1990
        DTF_LONG_CONDENSED          enum    DateTimeFormat ; Sun, Mar 5, 1990
        DTF_LONG_NO_WEEKDAY         enum    DateTimeFormat ; March 5th, 1990
        DTF_LONG_NO_WEEKDAY_CONDENSED enum  DateTimeFormat ; Mar 5, 1990
        DTF_SHORT                   enum    DateTimeFormat ; 3/5/90
        DTF_ZERO_PADDED_SHORT       enum    DateTimeFormat ; 03/05/90
        DTF_MD_LONG                 enum    DateTimeFormat ; Sunday, March 5th
        DTF_MD_LONG_NO_WEEKDAY      enum    DateTimeFormat ; March 5th
        DTF_MD_SHORT                enum    DateTimeFormat ; 3/5
        DTF_MY_LONG                 enum    DateTimeFormat ; March 1990
        DTF_MY_SHORT                enum    DateTimeFormat ; 3/90
        DTF_YEAR                    enum    DateTimeFormat ; 1990
        DTF_MONTH                   enum    DateTimeFormat ; March
        DTF_DAY                     enum    DateTimeFormat ; 5th
        DTF_WEEKDAY                 enum    DateTimeFormat ; Wednesday
            ;
        DTF_END_DATE_FORMATS        =   DateTimeFormat

        DTF_START_TIME_FORMATS      =   DateTimeFormat

        DTF_HMS                     enum    DateTimeFormat ; 1:05:31 PM
        DTF_HM                      enum    DateTimeFormat ; 1:05 PM
        DTF_H                       enum    DateTimeFormat ; 1 PM
        DTF_MS                      enum    DateTimeFormat ; 5:31
        DTF_HMS_24HOUR              enum    DateTimeFormat ; 13:05:31
        DTF_HM_24HOUR               enum    DateTimeFormat ; 13:05

        DTF_END_TIME_FORMATS        = DateTimeFormat

These date-time formats are generic. The examples shown to the right are for 
US based date-times only. In other countries, date-time formats may be 
substantially different.

**Library:** localize.def

----------
#### DBaseItem
    DBaseItem       struct
        DBI_group       word    ; The group in which the data resides.
        DBI_item        word    ; The item within that group.
    DBaseItem       ends

This structure defines a dbase item.

**Library:** cell.def

----------
#### DBGroupAndItem
    DBGroupAndItem          struct
        DBGI_item       word
        DBGI_group      word
    DBGroupAndItem          ends

**Library:** dbase.def

----------
#### dbptr
    dbptr   struct
        dbitem  word
        dbgroup word
    dbptr   ends

**Library:** config.def

----------
#### DCCFeatures
    DCCFeatures     record
        DCCF_DROP_CAP       :1
    DCCFeatures     end

These feature flags are used with ATTR_GEN_CONTROL_REQUIRE_UI and 
ATTR_GEN_CONTROL_PROHIBIT_UI.

**Library:** Objects/Text/tCtrlC.def

----------
#### DCCToolboxFeatures
    DCCToolboxFeatures      record
    DCCToolboxFeatures      end

**Library:** Objects/Text/tCtrlC.def

----------
#### DDFixed
    DDFixed struct
        DDF_frac        dword
        DDF_int     sdword
    DDFixed ends

This structure defines a 32 bit/32 bit fixed point number.

**Library:** geos.def

----------
#### DebugObjDuplicateResourceInfo
    DebugObjDuplicateResourceInfo   struct
        DODRI_tempOwner char    GEODE_NAME_SIZE dup(?)
        ; The permanent name of the geode owning the template that this block
        ; was duplicated from.
        DODRI_tempResource      word
        ; The handle of the template resource, unrelocated relative to the above.
    DebugObjDuplicateResourceInfo   ends

Info used to allow the debugger to print symbolic information for objects in 
the block this object lies in. The EC version of **ObjDuplicateResource**, after 
duplicating an object block, finds the first object & adds this piece of vardata 
to it, thereby tagging the block for the debugger with info regarding where it 
came from. The data stored is the unrelocated library, and unrelocated 
resource within that library, so that the entry is valid across sessions. The 
debugger, when displaying info about an object in a block that doesn't have a 
symbol, uses this to be able to show a reference like:

        ^h4020h:EditCopyTrigger

**Library:** metaC.def

----------
#### DefaultFieldNameUsage
    DefaultFieldNameUsage   etype   byte,   0
        DFNU_FIELD      enum    DefaultFieldNameUsage
        DFNU_COLUMN     enum    DefaultFieldNameUsage
        DFNU_FIXED      enum    DefaultFieldNameUsage

**Library:** impex.def

----------
#### Dependency
    Dependency      struct
        D_row           word        ; Row of the dependency.
        D_column        byte        ; Column of the dependency.
    Dependency      ends

**Library:** parse.def

----------
#### DependencyBlock
    DependencyBlock     struct
        DB_size     word    ; Size of the block containing the dependency.
    DependencyBlock     ends

**Library:** parse.def

----------
#### DependencyListHeader
    DependencyListHeader            struct
        DLH_next        dword   ; DBase item containing next block in the list.
    DependencyListHeader            ends

*DLH_next*. The "next" link must come first in this structure. It must be at the 
same position as the cells dependency list header (which must also fall at the 
start of the cell data). 

**Library:** parse.def

----------
#### DependencyParameters
    DependencyParameters            struct
        DP_common           CommonParameters <>
        ;
        ; Possible callbacks:
        ;   CT_CREATE_CELL, CT_EMPTY_CELL, CT_NAME_TO_CELL,
        ;   CT_FUNCTION_TO_TOKEN
        ;
        ;
        ; Everything else here is used exclusively by the dependency list code.
        ; Applications do not need to initialize it and should not depend on
        ; the values returned in this part of the stack frame.
        ;
        DP_dep          dword   ; Dbase item containing the dependency list.
        DP_prev         dword   ; Dbase item containing the previous block.
        DP_prevIsCell   byte    ; If non-zero, previous entry is the cell.
        DP_chunk        word    ; Chunk handle of the current dependency.
        align           word
    DependencyParameters            ends

This structure is passed to the dependency code.

**Library:** parse.def

----------
#### DerefFlags
    DerefFlags      record
        DF_DONT_POP_ARGUMENT    :1      ; Set = don't pop arg from arg stack.
    DerefFlags      end

**Library:** parse.def

----------
#### DestinationClassArgs
    DestinationClassArgs            struct
        DCA_class       fptr.ClassStruct
    DestinationClassArgs            ends

**Library:** Objects/genC.def

----------
#### DetachDataEntry
    DetachDataEntry     struct
        DDE_ackCount        word
        DDE_callerID        word
        DDE_ackOD           optr
        DDE_completeMsg     word ; Message to send to itself when ackCount goes to zero.
    DetachDataEntry     ends

**Library:** Objects/metaC.def

----------
#### DevicePresent
    DevicePresent       etype       word
        DP_NOT_PRESENT          enum        DevicePresent, 0xffff   
        DP_CANT_TELL            enum        DevicePresent, 0    
        DP_PRESENT              enum        DevicePresent, 1    
        DP_INVALID_DEVICE       enum        DevicePresent, 0xfffe

DP_NOT_PRESENT  
Driver knows that a device is not present.

DP_CANT_TELL  
Driver cannot determine if a device is present.

DP_PRESENT  
Driver knows that a device is present.

DP_INVALID_DEVICE  
An unknown device string was passed.

**Library:** driver.def

----------
#### DirPathInfo
    DirPathInfo     record
        DPI_EXISTS_LOCALLY          :1
        DPI_ENTRY_NUMBER_IN_PATH    :7
        DPI_STD_PATH                StandardPath:8
    DirPathInfo     end

DPI_EXISTS_LOCALLY  
File exists in directory under primary tree (usually a local directory, not a 
server-based one).

**Library:** file.def

----------
#### DiskCopyCallback
    DiskCopyCallback        etype       word, 0
        DCC_GET_SOURCE_DISK                 enum DiskCopyCallback
            ;   Desc:   prompt the user to insert the source disk.
            ;   Pass:
            ;       ax - DCC_GET_SOURCE_DISK
            ;       dl - 0 based drive number
            ;   Return:
            ;       ax = 0 to continue, non-0 to abort

        DCC_REPORT_NUM_SWAPS                enum DiskCopyCallback
            ;   Desc:   tell the user how many times to swap disks
            ;           in order to accomplish the copy.
            ;   Pass:
            ;       ax - DCC_REPORT_NUM_SWAPS
            ;       dx - number of swaps required
            ;   Return:
            ;       ax = 0 to continue, non-0 to abort

        DCC_GET_DEST_DISK                   enum DiskCopyCallback
            ;   Desc:   prompt the user to insert the destination disk.
            ;   Pass:
            ;       ax - DCC_GET_DEST_DISK
            ;       dl - 0 based drive number
            ;   Return:
            ;       ax = 0 to continue, non-0 to abort

        DCC_VERIFY_DEST_DESTRUCTION         enum DiskCopyCallback
            ;   Desc:   make sure the user really wants to biff the formatted disk
            ;           inserted as the destination.
            ;   Pass:
            ;       ax - DCC_VERIFY_DEST_DESTRUCTION
            ;       bx - disk handle of destination disk
            ;       dl - 0 based drive number
            ;   Return:
            ;       ax = 0 to continue, non-0 to abort

        DCC_REPORT_FORMAT_PCT               enum DiskCopyCallback
            ;   Desc:   During the formatting phase of the copy, report how much of
            ;           the format is complete.
            ;   Pass:
            ;       ax - DCC_REPORT_FORMAT_PCT
            ;       dx - percentage of destination disk formatted
            ;   Return:
            ;       ax = 0 to continue, non-0 to abort

        DCC_REPORT_READ_PCT                 enum DiskCopyCallback
            ;   Desc:   Report how much of the source disk has been read.
            ;   Pass:
            ;       ax - DCC_REPORT_READ_PCT
            ;       dx - percentage of source disk read
            ;   Return:
            ;       ax = 0 to continue, non-0 to abort

        DCC_REPORT_WRITE_PCT                enum DiskCopyCallback
            ;   Desc:   Report how much of the destination disk has been written.
            ;   Pass:
            ;       ax - DCC_REPORT_WRITE_PCT
            ;       dx - percentage of destination disk written
            ;   Return:
            ;       ax = 0 to continue, non-0 to abort

This type, when passed to the routine **DiskCopy**, specifies the type of 
callback operation to use with this routine.

**Library:** disk.def

----------
#### DiskCopyError
    DiskCopyError       etype       word, FormatError
        ERR_INVALID_SOURCE_DRIVE                            enum DiskCopyError
        ERR_INVALID_DEST_DRIVE                              enum DiskCopyError
        ERR_SOURCE_DRIVE_DOESNT_SUPPORT_DISK_COPY           enum DiskCopyError
        ERR_DEST_DRIVE_DOESNT_SUPPORT_DISK_COPY             enum DiskCopyError
        ERR_DRIVES_HOLD_DIFFERENT_FILESYSTEM_TYPES          enum DiskCopyError
        ERR_SOURCE_DISK_INCOMPATIBLE_WITH_DEST_DRIVE        enum DiskCopyError
        ERR_SOURCE_DISK_NOT_FORMATTED                       enum DiskCopyError
        ERR_COULD_NOT_REGISTER_FORMATTED_DESTINATION_DISK   enum DiskCopyError
        ERR_DISKCOPY_INSUFFICIENT_MEM                       enum DiskCopyError
        ERR_CANT_READ_FROM_SOURCE                           enum DiskCopyError
        ERR_CANT_WRITE_TO_DEST                              enum DiskCopyError
        ERR_OPERATION_CANCELLED                             enum DiskCopyError
        ERR_DISK_IS_IN_USE                                  enum DiskCopyError
        ERR_INVALID_SOURCE_DRIVE                            enum DiskCopyError

These enumerated types begin at the point where **FormatError** ends, so an 
error formatting the destination disk can be returned immediately without 
conversion.

ERR_INVALID_SOURCE_DRIVE  
The passed source drive doesn't exist.

ERR_INVALID_DEST_DRIVE  
The passed destination drive doesn't exist.

ERR_DRIVES_HOLD_DIFFERENT_FILESYSTEM_TYPES  
The two drives are managed by different file system drivers, so a disk cannot 
be copied between the two with this function.

ERR_SOURCE_DISK_INCOMPATIBLE_WITH_DEST_DRIVE  
The source disk is formatted in a manner that the destination drive does not 
support.

ERR_SOURCE_DISK_NOT_FORMATTED  
The source disk was not properly formatted.

ERR_COULD_NOT_REGISTER_FORMATTED_DESTINATION_DISK  
The copy operation attempted to register the source disk after prompting for 
it and the registration failed.

**Library:** disk.def

----------
#### DiskCopyFlags
    DiskCopyFlags           record
        DCF_GREEDY      :1
                        :7
    DiskCopyFlags           end

DCF_GREEDY  
If set, the copy operation will use as much memory as necessary and possible 
to minimize disk swaps. This flag is applicable only if source and destination 
drives are the same.

**Library:** disk.def

----------
#### DiskFindResult
    DiskFindResult      etype       word
        DFR_UNIQUE              enum    DiskFindResult
        DFR_NOT_UNIQUE          enum    DiskFindResult
        DFR_NOT_FOUND           enum    DiskFindResult

**Library:** disk.def

----------
#### DiskFormatFlags
    DiskFormatFlags     record
                                    :13
        DFF_CALLBACK_PCT_DONE       :1
        DFF_CALLBACK_CYL_HEAD       :1
        DFF_FORCE_ERASE             :1
    DiskFormatFlags     end

DFF_CALLBACK_PCT_DONE  
This flag is set if the disk format should call the callback with the 
percent-complete of the operation.

DFF_CALLBACK_CYL_HEAD  
This flag is set if the disk format should call the callback with the current 
cylinder and head.

DFF_FORCE_ERASE  
This flag is set if we wish to force erasure of the entire disk.

**Library:** disk.def

----------
#### DiskInfoStruct
    DiskInfoStruct      struct
        DIS_blockSize   word    ; number of bytes in which file system allocations
                                ; are performed. Useful as an efficient
                                ; buffer size for disk transfers, with
                                ; certain restrictions.
        DIS_freeSpace   sdword  ; number of bytes free on the disk.
        DIS_totalSpace  sdword  ; number of bytes on the entire disk.
        DIS_name        char    VOLUME_BUFFER_SIZE dup(?)
    DiskInfoStruct      ends

**Library:** disk.def

----------
#### DiskRestoreError
    DiskRestoreError        etype       word, 0, 1
        DRE_DISK_IN_DRIVE                       enum DiskRestoreError
             ;  This is the value returned by DiskRestore itself and the callback 
             ;  routine (if called at all) if the disk is in the drive.
             ;  In the callback's case, of course, it cannot be sure that the disk
             ;  is in the drive; it merely thinks it still is.
        DRE_DRIVE_NO_LONGER_EXISTS              enum DiskRestoreError
             ;  The drive in which the disk was registered no longer exists and the
             ;  file system driver either isn't loaded or couldn't restore the
             ;  drive.
        DRE_REMOVABLE_DRIVE_DOESNT_HOLD_DISK    enum DiskRestoreError
             ;  The disk was in a removable-media drive and that drive doesn't 
             ;  contain the disk.
        DRE_USER_CANCELED_RESTORE               enum DiskRestoreError
             ;  This type is solely for callback routines to use, as it implies the
             ;  user was asked, which DiskRestore will not do.
        DRE_COULDNT_CREATE_NEW_DISK_HANDLE      enum DiskRestoreError
             ;  The operation attempted to create the new disk handle after 
             ;  deciding the disk was in the drive, but had some difficulty finding
             ;  the disk name, etc.
        DRE_REMOVABLE_DRIVE_IS_BUSY             enum DiskRestoreError
             ;  The disk was in a removable-media drive that is currently marked
             ;  busy, so the system could neither confirm nor deny that it holds
             ;  the saved disk. Try again later.
        DRE_NOT_ATTACHED_TO_SERVER              enum DiskRestoreError
             ;  The disk was from a network server to which we are not logged in.
        DRE_PERMISSION_DENIED                   enum DiskRestoreError
             ;  The disk was on a network which is (now) denying access to it.
        DRE_ALL_DRIVES_USED                     enum DiskRestoreError
             ;  The disk was on a network volume that isn't mounted, but there is
             ;  no drive left to which it can be mapped.

**Library:** disk.def

----------
#### DisplayAspectRatio
    DisplayAspectRatio      etype       byte
        DAR_NORMAL              enum        DisplayAspectRatio  ;VGA, MCGA
        DAR_SQUISHED            enum        DisplayAspectRatio  ;EGA, HGCA
        DAR_VERY_SQUISHED       enum        DisplayAspectRatio  ;CGA

**Library:** win.def

----------
#### DisplayClass
    DisplayClass        etype       byte
        DC_TEXT     enum        DisplayClass    ;denotes that display is 
                                                ;character only (Not implemented)
        DC_GRAY_1   enum        DisplayClass    ;1 bit/pixel gray scale
        DC_GRAY_2   enum        DisplayClass    ;2 bit/pixel gray scale
        DC_GRAY_4   enum        DisplayClass    ;4 bit/pixel gray scale
        DC_GRAY_8   enum        DisplayClass    ;8 bit/pixel gray scale
        DC_COLOR_2  enum        DisplayClass    ;2 bit/pixel color index
        DC_COLOR_4  enum        DisplayClass    ;4 bit/pixel color index
        DC_COLOR_8  enum        DisplayClass    ;8 bit/pixel color index
        DC_CF_RGB   enum        DisplayClass    ;color with RGB values

**Library:** win.def

----------
#### DisplayScheme
    DisplayScheme       struct
        DS_colorScheme          ColorScheme     ;passed in al
        DS_displayType          DisplayType     ;passed in ah
        DS_unused               word            ;passed in bx
        DS_fontID               FontID          ;passed in cx
        DS_pointSize            sword           ;passed in dx
    DisplayScheme       ends

*DS_fontID* & *DS_pointSize* are conveniently set up so they'll be in registers 
**cx:dx** to conform to graphics routines.

**Library:** Objects/visC.def

----------
#### DisplaySize
    DisplaySize     etype       byte
        DS_TINY         enum    DisplaySize ;tiny screens: CGA, 256x320
        DS_STANDARD     enum    DisplaySize ;standard screens: EGA,VGA,HGC,MCGA
        DS_LARGE        enum    DisplaySize ;large screens: 800x600 SVGA
        DS_HUGE         enum    DisplaySize ;huge screens

**Library:** win.def

----------
#### DisplayType
    DisplayType     record
        DT_DISP_SIZE                DisplaySize:2
        DT_DISP_ASPECT_RATIO        DisplayAspectRatio:2
        DT_DISP_CLASS               DisplayClass:4
    DisplayType     end

DT_DISP_SIZE  
Size of display.

DT_DISP_ASPECT_RATIO  
Aspect ratio of display.

DT_DISP_CLASS  
Class of driver (or closest match).

**Library:** win.def

----------
#### DistanceUnit
    DistanceUnit        etype       byte
        DU_POINTS                   enum DistanceUnit
            ;U.S. points (72 per inch)
            ;Display format is "###.### pt"
            ;Entry format is "###.### pt"
        DU_INCHES                   enum DistanceUnit
            ;Display format is "##.### in"
            ;Entry format is "##.### in" or "##.###"" (double quotes = inches)
        DU_CENTIMETERS              enum DistanceUnit
            ;Display format is "###.### cm"
            ;Entry format is "###.### cm"
        DU_MILLIMETERS              enum DistanceUnit
            ;Display format is "###.### mm"
            ;Entry format is "###.### mm"
        DU_PICAS                    enum DistanceUnit                       ;U.S. picas (12 points)
            ;Display format is "###.### pi"
            ;Entry format is "###.### pi"
        DU_EUR_POINTS               enum DistanceUnit                       ;European points
            ;Display format is "###.### ep"
            ;Entry format is "###.### ep"
        DU_CICEROS                  enum DistanceUnit
            ;Display format is "###.### ci" (should fraction be in e points ???)
            ;Entry format is "###.### ci"
        DU_POINTS_OR_MILLIMETERS    enum DistanceUnit
            ;Depends on units for app
        DU_INCHES_OR_CENTIMETERS    enum DistanceUnit
            ;Depends on units for app

        LOCAL_DISTANCE_BUFFER_SIZE  = 32

**Library:** localize.def

----------
#### DocQuitStatus
    DocQuitStatus       etype       word
        DQS_OK              enum    DocQuitStatus
        DQS_CANCEL          enum    DocQuitStatus
        DQS_DELAYED         enum    DocQuitStatus
        DQS_SAVE_ERROR      enum    DocQuitStatus

**Library:** Objects/gDocGrpC.def

----------
#### DocumentCommonParams
    DocumentCommonParams            struct
        DCP_name            FileLongName
        DCP_diskHandle      word
        DCP_path            PathName
        DCP_docAttrs        GenDocumentAttrs
        DCP_flags           DocumentOpenFlags
        DCP_connection      IACPConnection  ; OPEN_DOC & SEARCH_FOR_DOC only: IACP
                                            ; connection that requested the open.
                                            ; 0 if open requested by the user.
    DocumentCommonParams            ends

Not all of these flags are always needed, but they are all in one structure to 
simplify passing the flags around.

**Library:** Objects/gDocC.def

----------
#### DocumentFileChangedParams
    DocumentFileChangedParams               struct
        DFCP_name               FileLongName
        DFCP_diskHandle         hptr
        DFCP_path               PathName
        DFCP_display            optr
        DFCP_document           optr
    DocumentFileChangedParams               ends

**Library:** Objects/gDocCtrl.def

----------
#### DocumentOpenFlags
    DocumentOpenFlags       record
        DOF_CREATE_FILE_IF_FILE_DOES_NOT_EXIST      :1
        DOF_FORCE_TEMPLATE_BEHAVIOR                 :1
        DOF_SAVE_AS_OVERWRITE_EXISTING_FILE         :1
        DOF_REOPEN                                  :1
        DOF_RAISE_APP_AND_DOC                       :1
        DOF_FORCE_REAL_EMPTY_DOCUMENT               :1
        DOF_OPEN_FOR_IACP_ONLY                      :1
        DOF_NO_ERROR_DIALOG :1                      :1
        DOF_NO_DOC_SEARCH                           :1
    DocumentOpenFlags       end

Flags used for document OPEN operations:

DOF_CREATE_FILE_IF_FILE_DOES_NOT_EXIST  
This bit controls behavior when the file to open does not exist. 
Setting this bit causes a new file to be created if this case.  
DOF_FORCE_TEMPLATE_BEHAVIOR  
Forces the document to be treated as a template

Flags used for document SAVE AS operations:

DOF_SAVE_AS_OVERWRITE_EXISTING_FILE  
This bit controls behavior when the file to be Save As'd to 
exists. Setting this bit causes the file to be overwritten.  
DOF_REOPEN  
We are re-opening the file.

Flags used for document SEARCH_FOR_DOC operations:

DOF_RAISE_APP_AND_DOC  
Raise the application & document to the top, too, regardless of 
non-zero nature of DCP_connection.

The following two fields are used when creating new documents.

DOF_FORCE_REAL_EMPTY_DOCUMENT  
DOF_OPEN_FOR_IACP_ONLY  
Set if document was opened for purpose of handling an IACP 
request only. If user opens in the meantime, this bit is cleared.

The following two fields are Internal, and you should try to ignore them.

DOF_NO_ERROR_DIALOG  
Internal.  
DOF_NO_DOC_SEARCH  
Internal.

**Library:** Objects/gDocC.def

----------
#### DosCodePage
    DosCodePage     etype       word
        CODE_PAGE_US                enum        DosCodePage, 437
        CODE_PAGE_MULTILINGUAL      enum        DosCodePage, 850
        CODE_PAGE_MULTILINGUAL_EURO enum        DosCodePage, 858
        CODE_PAGE_PORTUGUESE        enum        DosCodePage, 860
        CODE_PAGE_CANADIAN_FRENCH   enum        DosCodePage, 863
        CODE_PAGE_NORDIC            enum        DosCodePage, 865
        CODE_PAGE_SJIS              enum        DosCodePage, 932

**Library:** localize.def

----------
#### DosExecFlags
    DosExecFlags        record
        DEF_PROMPT                  :1
        DEF_FORCED_SHUTDOWN         :1
        DEF_INTERACTIVE             :1
        DEF_INTERACTIVE             :1
        DEF_SWAP_EXEC               :1
        DEF_SWAP_TSR                :1
        DEF_MEM_REQ                 :1 
                                    :2
    DosExecFlags        end

DEF_PROMPT  
Set if we want to prompt the user to strike a key to return to GEOS.

DEF_FORCED_SHUTDOWN  
Set if we want to force the user to shutdown (cannot abort, program must be 
run).

DEF_INTERACTIVE  
Set if program being run is interactive shell and we should change $PROMPT 
to tell the user to type "exit" to return to GEOS.

DEF_SWAP_EXEC  
Set if GEOS should be swapped out instead of shutdown.

DEF_SWAP_TSR  
Set if GEOS should swap itself out and TSR, without executing 
the program.

DEF_MEM_REQ  
Set if a **DosExecArgAndMemReqStruct** is passed to 
**DosExec** in **es:di** instead of the argument string

**Library:** system.def

----------
#### DosExecMemReq
    DosExecMemReq   struct
        DEMR_minimum    word        ; minimum memory requirement
        DEMR_optimal    word        ; optimal memory requirement
    DosExecMemReq   ends

**Library:** 

----------
#### DosExecMemReqsStruct
    DosExecMemReqsStruct        struct
        DEMRS_tsr           BooleanByte     BB_FALSE    ; program is a TSR
        DEMRS_conventional  DosExecMemReq   <0,0>       ; conventional
        DEMRS_upper         DosExecMemReq   <0,0>       ; upper
        DEMRS_ems           DosExecMemReq   <0,0>       ; EMS
        DEMRS_xms           DosExecMemReq   <0,0>       ; XMS
        DEMRS_extended      DosExecMemReq   <0,0>       ; raw extended
    DosExecMemReqsStruct        ends

**Library:** 

----------
#### DrawFlags
    DrawFlags       record
        DF_EXPOSED              :1
        DF_OBJECT_SPECIFIC      :1
        DF_PRINT                :1
        DF_DONT_DRAW_CHILDREN   :1
        DF_DISPLAY_TYPE         DisplayClass:4
    DrawFlags       end

DF_EXPOSED  
Set if the current draw is the result of a MSG_META_EXPOSED being 
processed; if this is the case, we are in-between calls made to the window 
system of **GrBeginUpdate** and **GrEndUpdate**.

DF_OBJECT_SPECIFIC  
This bit is used differently under different objects.

For scrolling list objects:

This flag is set if a composite which controls the drawing of its own children 
should *not* draw its children. i.e. the draw should act on only the composite 
itself.

For text objects:

This flag is used by the text object to draw all of its lines, not just the ones 
which aren't masked out. This is used to get rotated text up on the screen.

DF_PRINT  
This flag is set if the draw is a result of a MSG_META_EXPOSED_FOR_PRINT. 
If this bit is set then DF_EXPOSED will be set also.

DF_DONT_DRAW_CHILDREN  
This flag is set if composites should not be drawing their children.

DF_DISPLAY_TYPE  
This flag is set to **DisplayClass** (*not* **DisplayType**).

**Library:** Objects/visC.def

----------
#### DrawMonikerFlags
    DrawMonikerFlags        record
        DMF_TEXT_ONLY               :1
        DMF_UNDERLINE_ACCELERATOR   :1
        DMF_CLIP_TO_MAX_WIDTH       :1
        DMF_NONE                    :1
        DMF_Y_JUST                  Justification:2
        DMF_X_JUST                  Justification:2
    DrawMonikerFlags        end

DMF_TEXT_ONLY  
Set if we can only draw a text moniker on this object.

DMF_UNDERLINE_ACCELERATOR  
Underlines accelerator key if set.

DMF_CLIP_TO_MAX_WIDTH  
Causes the moniker to be clipped to its maximum width (passed elsewhere, 
if used).

DMF_NONE  
TRUE to draw at current pen position.

DMF_Y_JUST  
Vertical justification.

DMF_X_JUST  
Horizontal justification.

**Library:** Objects/visC.def

----------
#### DriveExtendedStatus
    DriveExtendedStatus             record
        DES_LOCAL_ONLY      :1
        DES_READ_ONLY       :1
        DES_FORMATTABLE     :1
        DES_ALIAS           :1
        DES_BUSY            :1
        DES_EXTERNAL        DriveStatus:8
    DriveExtendedStatus             end

DES_LOCAL_ONLY  
Set if device cannot be viewed over a network.

DES_READ_ONLY  
Set if device is read-only. 

DES_FORMATTABLE  
Set if disks can be formatted in the drive. If set, implies disks can be copied 
in the drive using **DiskCopy**.

DES_ALIAS  
Set if drive is actually an alias for a path on another drive.

DES_BUSY  
Set if drive will be busy for an extended period of time (e.g. when disk is being 
formatted). 

DES_EXTERNAL  
Externally-visible status flags.

**Library:** drive.def

----------
#### DriverAttrs
    DriverAttrs         record
        DA_FILE_SYSTEM              :1
        DA_CHARACTER                :1
        DA_HAS_EXTENDED_INFO        :1
                                    :13
        DriverAttrs     end

DA_FILE_SYSTEM  
Driver is used primarily for file access.

DA_CHARACTER  
Driver is used primarily with character-oriented devices.

DA_HAS_EXTENDED_INFO  
Driver has **DriverExtendedInfo** structure, with attendant mandatory 
functions.

**Library:** driver.def

----------
#### DriverEscCode
    DriverEscCode       etype   word, 8000h, 1
        DRV_ESC_QUERY_ESC   enum    DriverEscCode   ; query for escape cpde 
                                                    ; support

**Library:** driver.def

----------
#### DriverExtendedFunction
    DriverExtendedFunction      etype   word, DriverFunction, 2
        DRE_TEST_DEVICE     enum    DriverExtendedFunction
        ;   PASS:       dx:si   = pointer to null-terminated device name string
        ;   RETURN:     ax  = DevicePresent
        ;               carry set if DP_INVALID_DEVICE, clear otherwise
        ;   DESTROYS:   di
        ;
        ;   This function tests the existence of a particular device the driver
        ;   supports.

        DRE_SET_DEVICE      enum    DriverExtendedFunction
        ;   PASS:       dx:si   = pointer to null-terminated device name string
        ;   RETURN:     nothing
        ;   DESTROYS:   di
        ;
        ;   This function informs the driver which of its many devices it is to
        ;   support.

**Library:** driver.def

----------
#### DriverExtendedInfoStruct
    DriverExtendedInfoStruct            struct
        DEIS_common             DriverInfoStruct
        DEIS_resource           hptr.DriverExtendedInfoTable
    DriverExtendedInfoStruct            ends

This structure is used by preferences to locate the names of devices supported 
by a particular driver.

An extended driver is one that can handle multiple types of devices, 
identified by ASCII strings that the driver provides. The specific device to be 
supported is specified by a DRE_SET_DEVICE call after the driver is loaded.

*DEIS_common* stores the regular driver information within a 
**DriverInfoStruct**.

*DEIS_resource* stores the resource containing additional driver information. 
The resource must be sharable so other geodes can lock it down. It should 
also be read-only.The **DriverExtendedInfoTable** is stored in a separate 
resource to keep the amount of fixed memory required to a minimum.

**Library:** driver.def

----------
#### DriverExtendedInfoTable
    DriverExtendedInfoTable         struct
        DEIT_common             LMemBlockHeader
        DEIT_numDevices         word
        DEIT_nameTable          nptr.lptr.char
        DEIT_infoTable          nptr.word
    DriverExtendedInfoTable         ends

*DEIT_common* stores the common LMem header info. Just use {} to define 
this field and Esp will skip this field when defining the structure.

*DEIT_numDevices* stores the number of device types supported by the driver

*DEIT_nameTable* stores the pointer to the table of *DEIT_numDevices* pointers 
to the device names themselves. As indicated by this being an nptr, the table 
and the names lie in this same resource.

*DEIT_infoTable* stores the pointer to the table of *DEIT_numDevices*, with 
extra words containing driver-specific data for each device. The type of data 
stored here is specified by the driver-type-specific interface .def file (e.g. 
**mouseDriver.def**).

**Library:** driver.def

----------
#### DriverFunction
    DriverFunction      etype       word, 0, 2
        DR_INIT             enum DriverFunction ;Initialize driver
        ;   PASS:   cx  = di passed to GeodeLoad. Garbage if loaded via
        ;            GeodeUseDriver
        ;           dx  = bp passed to GeodeLoad. Garbage if loaded via
        ;            GeodeUseDriver
        ;   RETURN: carry set if driver initialization failed. Driver will be
        ;           unloaded by the system.
        ;           carry clear if initialization successful.
        ;
        ;   DESTROYS:   bp, ds, es, ax, di, si, cx, dx
        ;

        DR_EXIT             enum DriverFunction ;Exit driver
        ;   PASS:       nothing
        ;   RETURN:     nothing
        ;   DESTROYS:   ax, bx, cx, dx, si, di, ds, es
        ;
        ;   NOTES:  If the driver has GA_SYSTEM set, the handler for this function
        ;       must be in fixed memory and may not use anything in movable
        ;       memory.

        DRIVER_SUSPEND_ERROR_BUFFER_SIZE     equ     128

        DR_SUSPEND          enum DriverFunction
        ;   SYNOPSIS:   Prepare the device for going into stasis while GEOS
        ;               is task-switched out. Typical actions include disabling
        ;               interrupts or returning to text-display mode.
        ;
        ;   PASS:       cx:dx   = buffer in which to place reason for refusal, if
        ;                suspension refused (DRIVER_SUSPEND_ERROR_BUFFER_SIZE
        ;                bytes long)
        ;   RETURN:     carry set if suspension refused:
        ;               cx:dx   = buffer filled with null-terminated reason,
        ;                standard GEOS character set.
        ;               carry clear if suspension approved
        ;   DESTROYS:   ax, di
        ;

    DR_UNSUSPEND            enum DriverFunction
        ;   SYNOPSIS:   Reconnect to the device when GEOS is task-switched
        ;               back in.
        ;
        ;   PASS:       nothing
        ;   RETURN:     nothing
        ;   DESTROYS:   ax, di
        ;
        ; Protocol number for "DriverFunction" interface. All other driver protocols
        ; will be based on this number.
        ;
        DRIVER_PROTO_MAJOR          equ 2
        DRIVER_PROTO_MINOR          equ 0

**Library:** driver.def

----------
#### DriverInfoStruct
    DriverInfoStruct            struct
        DIS_strategy                fptr.far
        DIS_driverAttributes        DriverAttrs
        DIS_driverType              DriverType
        DriverInfoStruct        ends

This structure defines the characteristics of a particular driver. In general, 
applications will not need to access this structure unless they use a driver 
directly.

*DIS_strategy* stores the address of the strategy routine which calls the proper 
driver.

*DIS_driverAttributes* stores the device attributes for the driver.

*DIS_driverType* stores the type of driver (video, stream, printer, etc.).

**Library:** driver.def

----------
#### DriverType
    DriverType      etype       word, 1
        DRIVER_TYPE_VIDEO               enum        DriverType
        DRIVER_TYPE_INPUT               enum        DriverType
        DRIVER_TYPE_MASS_STORAGE        enum        DriverType
        DRIVER_TYPE_STREAM              enum        DriverType
        DRIVER_TYPE_FONT                enum        DriverType
        DRIVER_TYPE_OUTPUT              enum        DriverType
        DRIVER_TYPE_LOCALIZATION        enum        DriverType
        DRIVER_TYPE_FILE_SYSTEM         enum        DriverType
        DRIVER_TYPE_PRINTER             enum        DriverType
        DRIVER_TYPE_SWAP                enum        DriverType
        DRIVER_TYPE_POWER_MANAGEMENT    enum        DriverType
        DRIVER_TYPE_TASK_SWITCH         enum        DriverType
        DRIVER_TYPE_NETWORK             enum        DriverType
        DRIVER_TYPE_SOUND               enum        DriverType
        DRIVER_TYPE_PAGER               enum        DriverType
        DRIVER_TYPE_PCMCIA              enum        DriverType
        DRIVER_TYPE_FEP                 enum        DriverType

**Library:** driver.def

----------
#### DriveStatus
    DriveStatus     record
        DS_PRESENT          :1          ; Set if physical drive exists
        DS_MEDIA_REMOVABLE  :1          ; Set if disk can be removed from 
                                        ; the drive.
        DS_NETWORK          :1          ; Set if drive is on the network (or 
                                        ; accessed via network protocols), 
                                        ; so disk cannot be formatted or 
                                        ; copied.
                            :1
        DS_TYPE             DriveType:4 ; Type of drive
    DriveStatus     end

**Library:** drive.def

----------
#### DriveType
    DriveType       etype       byte
        DRIVE_5_25          enum    DriveType
        DRIVE_3_5           enum    DriveType
        DRIVE_FIXED         enum    DriveType
        DRIVE_RAM           enum    DriveType
        DRIVE_CD_ROM        enum    DriveType
        DRIVE_8             enum    DriveType
        DRIVE_PCMCIA        enum    DriveType
        DRIVE_UNKNOWN       enum    DriveType, 0xf

**Library:** drive.def

----------
#### DTCFeatures
    DTCFeatures     record
        DTCF_LIST       :1
        DTCF_CUSTOM     :1
    DTCFeatures     end

These features flags (used with ATTR_GEN_CONTROL_REQUIRE_UI and 
ATTR_GEN_CONTROL_PROHIBIT_UI).

**Library:** Objects/Text/tCtrlC.def

----------
#### DTCToolboxFeatures
    DTCToolboxFeatures      record
    DTCToolboxFeatures      end

**Library:** Objects/Text/tCtrlC.def

----------
#### DWFixed
    DWFixed     struct
        DWF_frac        word
        DWF_int         sdword
    DWFixed     ends

This structure stores a 32 bit/16 bit fixed point number.

**Library:** geos.def

----------
#### ElementArrayHeader
    ElementArrayHeader      struct
        EAH_meta        ChunkArrayHeader
        EAH_freePtr     word
    ElementArrayHeader      ends

This structure must be at the front of every element array. Since element 
arrays are special kinds of chunk arrays, the **ElementArrayHeader** must 
itself begin with a **ChunkArrayHeader**.

*EAH_meta* stores the **ChunkArrayHeader**.

*EAH_freePtr* stores the first free element within the element array. 
Applications should not examine or change this field.

**Library:** chunkarr.def

----------
#### EMCDetachData
    EMCDetachData   struct
        EMCDD_ackEvent      hptr 
        EMCDD_childBlock    hptr 
    EMCDetachData   ends

*EMCDD_ackEvent*  
Recorded MSG_META_ACK that will be sent back by everyone 
on the EXPRESS_MENU_CHANGE list.

*EMCDD_childBlock*  
Handle of child block to be freed when 
MSG_META_DETACH_COMPLETE comes in, saying that 
everyone who could possibly care has acknowledged the detach, 
so it's safe to actually free the child block, which the 
GenControl object won't do when it receives a 
MSG_META_DETACH.

**Library:** eMenuC.def

----------
#### EMCFeatures
    EMCFeatures     record
        EMCF_GEOS_TASKS_LIST        :1
        EMCF_DESK_ACCESSORY_LIST    :1
        EMCF_MAIN_APPS_LIST         :1
        EMCF_OTHER_APPS_LIST        :1
        EMCF_CONTROL_PANEL          :1
        EMCF_DOS_TASK_LIST          :1
        EMCF_UTILITIES              :1
        EMCF_EXIT_TO_DOS            :1
    EMCFeatures     end

**Library:** eMenuC.def

----------
#### EmptyRowBlock
    EmptyRowBlock       struct
        ERB_header          LMemBlockHeader <>
        ERB_handles         word N_ROWS_PER_ROW_BLOCK dup (0)
    EmptyRowBlock       ends

An empty row block is an LMem block with space for several handles.

**Library:** cell.def

----------
#### EndCreatePassFlags
    EndCreatePassFlags      record
        ECPF_ADJUSTED_CREATE                :1
    EndCreatePassFlags      end

ECPF_ADJUSTED_CREATE  
The UIFA_ADJUST flag was set in the START_SELECT operation that began 
the object creation.

**Library:** grobj.def

----------
#### EndCreateReturnFlags
    EndCreateReturnFlags            record
        ECRF_NOT_CREATING       :1      ;object was not in create mode.
        ECRF_DESTROYED          :1      ;object was destroyed
    EndCreateReturnFlags            end

**Library:** grobj.def

----------
#### EndOfSongFlags
    EndOfSongFlags      record
        EOSF_UNLOCK             :1 = 0  ; unlock block at EOS?
        EOSF_DESTROY            :1 = 0  ; destroy sound at EOS?
                                :6
    EndOfSongFlags      end

**Library:** sound.def

----------
#### EnsureActiveFTPriorityPreferenceData
    EnsureActiveFTPriorityPreferenceData    struct
        EAFTPPD_priority        word
        EAFTPPD_avoidOptr       optr
    EnsureActiveFTPriorityPreferenceData    ends

**Library:** 

----------
#### EnsureNoMenusInStayUpModeParams
    EnsureNoMenusInStayUpModeParams struc
        ENMISUMP_menuCount      word    ;Number of menus released so far
    EnsureNoMenusInStayUpModeParams ends

**Library:** ui.def

----------
#### EntryPointRelocation
    EntryPointRelocation            struct
        EPR_geodeName           char GEODE_NAME_SIZE dup (?)
        EPR_entryNumber         word
    EntryPointRelocation            ends

This structure is passed to **ObjRelocateEntryPoint**.

**Library:** object.def

----------
#### EnvelopeOrientation
    EnvelopeOrientation         etype       byte, 0, 1
        EO_PORTAIT                  enum            EnvelopeOrientation
        EO_LANDSCAPE                enum            EnvelopeOrientation

**Library:** print.def

----------
#### ErrorCheckingFlags
    ErrorCheckingFlags      record
        ECF_UNUSED_1:1 
        ECF_UNUSED_2:1 
        ECF_ANAL_VMEM:1
        ECF_FREE:1              ;Ensure that all free blocks are 0xcccc
        ECF_HIGH:1              ; lot of random extra checking (old NORMAL)
        ECF_LMEM:1              ;Internal lmem checking
        ECF_BLOCK_CHECKSUM:1    ;Checksum on a particular block
        ECF_GRAPHICS:1          ;Misc graphics stuff
        ECF_SEGMENT:1           ;Extensive segment checking
        ECF_NORMAL:1            ;Misc kernel error checking
        ECF_VMEM:1              ;VM file consistency
        ECF_APP:1               ;Application error checking (if implemented
                                ;by applications)
        ECF_LMEM_MOVE:1         ;Force lmem blocks to move whenever possible
        ECF_UNLOCK_MOVE:1       ;Force unlocked blocks to move
        ECF_VMEM_DISCARD:1      ;Force clean VM blocks to be discarded
    ErrorCheckingFlags      end

**Library:** ec.def

----------
#### EvalErrorData
    EvalErrorData       struct
        EED_errorCode           ParserScannerEvaluatorError
    EvalErrorData       ends

**Library:** parse.def

----------
#### EvalFlags
    EvalFlags       record
        EF_MAKE_DEPENDENCIES    :1  ; Make dependencies instead of 
                                    ; recalculating
        EF_ONLY_NAMES           :1  ; Only name dependencies
        EF_KEEP_LAST_CELL       :1  ; Don't dereference the last cell
        EF_NO_NAMES             :1  ; Only non-name dependencies
        ;
        ; This flag is set inside the evaluator and shouldn't be used by
        ; applications.
        ;
        EF_ERROR_PUSHED         :1  ; Set: if an error was pushed on the arg 
                                    ; stack
                                :3
    EvalFlags       end

**Library:** parse.def

----------
#### EvalFunctionData
    EvalFunctionData        struct
        EFD_functionID          FunctionID  ; Function ID if a function
        EFD_nArgs               word        ; Number of arguments
    EvalFunctionData        ends

**Library:** parse.def

----------
#### EvalNameData
    EvalNameData        struct
        END_name            word
    EvalNameData        ends

**Library:** parse.def

----------
#### EvalOperatorData
    EvalOperatorData        struct
        EOD_opType      OperatorType            ; Type of operator
    EvalOperatorData        ends

**Library:** parse.def

----------
#### EvalParameters
    EvalParameters      struct
        EP_common               CommonParameters <>
        ;
        ; Possible callbacks:
        ;   CT_LOCK_NAME, CT_LOCK_FUNCTION, CT_UNLOCK
        ;
        EP_flags                EvalFlags <>    ; Evaluator flags
        ;
        ; Everything below this point is initialized by the Evaluator.
        ;
        EP_fpStack              word            ; Floating point stack pointer
        EP_depHandle            word            ; Block handle of dependency block
        EP_nestedLevel          word            ; Levels of nesting

        EVAL_MAX_NESTED_LEVELS  = 32
        EP_nestedAddresses      dword EVAL_MAX_NESTED_LEVELS dup (?)
        align                   word
    EvalParameters      ends

This structure provides information when the evaluator is invoked. This 
structure is passed in a stack frame.

**Library:** parse.def

----------
#### EvalRangeData
    EvalRangeData       struct
        ERD_firstCell           CellReference <>
        ERD_lastCell            CellReference <>
    EvalRangeData       ends

**Library:** parse.def

----------
#### EvalStackArgumentData
    EvalStackArgumentData           union
        ESAD_string         EvalStringData
        ESAD_range          EvalRangeData
        ESAD_error          EvalErrorData
    EvalStackArgumentData           end

**Library:** parse.def

----------
#### EvalStackArgumentType
    EvalStackArgumentType           record
        ESAT_EMPTY      :1          ; Set: Argument came from an empty cell
        ;
        ; Only one of the following will ever be set at a time for
        ; arguments on the evaluator argument stack.
        ;
        ESAT_ERROR      :1              ; Set: Argument is an error
        ESAT_RANGE      :1              ; Set: Argument is a range
        ESAT_STRING     :1              ; Set: Argument is a string
        ESAT_NUMBER     :1              ; Set: Argument is a number
                        :1
        ;
        ; Numbers have some possible sub-types
        ;
        ESAT_NUM_TYPE   NumberType:2    ; The type of the number
    EvalStackArgumentType           end

**Library:** parse.def

----------
#### EvalStackOperatorData
    EvalStackOperatorData           union
        ESOD_operator           EvalOperatorData
        ESOD_function           EvalFunctionData
    EvalStackOperatorData           end

**Library:** parse.def

----------
#### EvalStackOperatorType
    EvalStackOperatorType           etype       byte, 0, 1
        ESOT_OPERATOR           enum        EvalStackOperatorType
        ESOT_FUNCTION           enum        EvalStackOperatorType
        ESOT_OPEN_PAREN         enum        EvalStackOperatorType
        ESOT_TOP_OF_STACK       enum        EvalStackOperatorType

**Library:** parse.def

----------
#### EvalStringData
    EvalStringData          struct
        ESD_length      word    ; Length of the string. (String data follows.)
    EvalStringData          ends

**Library:** parse.def

----------
#### EvaluatePositionNotes
    EvaluatePositionNotes       record
        EPN_PADDING                 :14
        EPN_SELECTION_LOCK_SET      :1  ;Object's selection lock is set.
        EPN_BLOCKS_LOWER_OBJECTS    :1  ;GrObj blocks, covers up or otherwise 
                                        ;completely obscures objects
                                        ;underneath it at the position
    EvaluatePositionNotes       end

**Library:** grobj.def

----------
#### EvaluatePositionRating
    EvaluatePositionRating          etype       byte, 0
        EVALUATE_NONE               enum        EvaluatePositionRating
        EVALUATE_SUB_LOW            enum        EvaluatePositionRating
        EVALUATE_LOW                enum        EvaluatePositionRating
        EVALUATE_SUB_MEDIUM         enum        EvaluatePositionRating
        EVALUATE_MEDIUM             enum        EvaluatePositionRating
        EVALUATE_SUB_HIGH           enum        EvaluatePositionRating
        EVALUATE_HIGH               enum        EvaluatePositionRating

EVALUATE_NONE  
Point is not interesting at all.

EVALUATE_SUB_LOW  
This type is not currently used.

EVALUATE_LOW  
Point is in a rectangle bounding an object.

EVALUATE_SUB_MEDIUM  
This type is not currently used.

EVALUATE_MEDIUM  
Point is inside an enclosed but not a filled object.

EVALUATE_SUB_HIGH  
This type is not currently used.

EVALUATE_HIGH  
Point is on a line or a filled - or partially filled - area.

**Library:** grobj.def

----------
#### ExitFlags
    ExitFlags       record
        EF_PANIC        :1  ; Set if the exit is unstable; in this case we 
                            ; choose not to write out the .ini file.
        EF_RUN_DOS      :1  ; Set if we should run a DOS program upon exit
        EF_OLD_EXIT     :1  ; Set if we should use old-style (int 20h) exit
                            ; call (if accidentally run under DOS 1.x).
        EF_RESET        :1  ; Set if we should reset the machine instead of
                            ; exiting.
        EF_RESTART      :1  ; Set if should reload GEOS at the end.
    ExitFlags       end

**Library:** system.def

----------
#### ExportControlAttrs
    ExportControlAttrs      record
        ECA_IGNORE_INPUT        :1      ; ignore input while export occurs
                                :15
    ExportControlAttrs      end

**Library:** impex.def

----------
#### ExportControlFeatures
    ExportControlFeatures       record
        EXPORTCF_EXPORT_TRIGGER :1  ; export trigger
        EXPORTCF_FORMAT_OPTIONS :1  ; export format UI parent, under which is 
                                    ; placed any UI specific to the currently
                                    ; selected format
        EXPORTCF_BASIC          :1  ; export file selector, export format list,
                                    ; export file name, and export app UI parent,
                                    ; under which is placed any UI specific to 
                                    ; the app
        EXPORTCF_GLYPH          :1  ; glyph at top of export dialog box
    ExportControlFeatures       end

These Feature flags are used with ATTR_GEN_CONTROL_REQUIRE_UI and 
ATTR_GEN_CONTROL_PROHIBIT_UI.

**Library:** impex.def

----------
#### ExportControlToolboxFeatures
    ExportControlToolboxFeatures                record
        EXPORTCTF_DIALOG_BOX                :1
    ExportControlToolboxFeatures                end

**Library:** impex.def

----------
#### ExtSelFlags
    ExtSelFlags     record
        ESF_INITIAL_SELECTION       :1  ;set if initial selection -- will update
                                        ; all items between anchor and extent
        ESF_XOR_INDIVIDUAL_ITEMS    :1  ;whether xoring changed items. If
                                        ; clear, sets items within new 
                                        ; selection, clears others.
        ESF_CLEAR_UNSELECTED_ITEMS  :1  ;set to clear non-selected items.
        ESF_SELECT                  :1  ;set when items in the selection 
                                        ; should be turned on, rather than
                                        ; off.
                                    :4
    ExtSelFlags     end

**Library:** Objects/gItemGC.def

----------
#### FACFeatures
    FACFeatures         record
        FACF_FONT_WEIGHT            :1
        FACF_FONT_WIDTH             :1
        FACF_TRACK_KERNING          :1
    FACFeatures         end

**Library:** Objects/Text/tCtrlC.def

----------
#### FACToolboxFeatures
    FACToolboxFeatures      record
    FACToolboxFeatures      end

**Library:** Objects/Text/tCtrlC.def

----------
#### FatalErrors
    FatalErrors     etype   word, 0
        CAN_NOT_USE_CHUNKSIZEPTR_MACRO_ON_EMPTY_CHUNKS      enum FatalErrors
        CHUNK_ARRAY_BAD_ELEMENT                             enum FatalErrors
        MACRO_REQUIRES_FIXED_SIZE_ELEMENTS                  enum FatalErrors
        CANNOT_USE_DBCS_IN_THIS_VERSION                     enum FatalErrors
        ; Double Byte characters (DBCS) are not supported in this version of PC/GEOS

**FatalErrors** is an enumerated type into which named error codes are placed. 
The members placed in the type should be accessible to all modules of a 
patient or be assigned ranges by the programmer. That makes no difference 
to Swat. NOTE: definition must lie outside the StartKernel/EndKernel 
bounds if Swat is to receive the type unmolested.

**FatalErrors** are errors global to the entire system.

**Library:** ec.def

----------
#### FCFeatures
    FCFeatures      record
        FCF_SHORT_LIST          :1
        FCF_LONG_LIST           :1
    FCFeatures      end

**Library:** Objects/Text/tCtrlC.def

----------
#### FCToolboxFeatures
    FCToolboxFeatures           record
        FCTF_TOOL_LIST      :1
    FCToolboxFeatures           end

**Library:** Objects/Text/tCtrlC.def

----------
#### FEDosInfo
    FEDosInfo       struct
        FEDI_attributes     FileAttrs           ; file's attributes
        FEDI_modified       FileDateAndTime     ; file's modification timestamp
        FEDI_fileSize       dword               ; file's size in bytes
        FEDI_name           FileLongName        ; file's name and extension in 
                                                ; the form of a null terminated 
                                                ; string
        FEDI_pathInfo       DirPathInfo
    FEDosInfo       ends

This structure is used with FileEnum.

**Library:** fileEnum.def

----------
#### FENameAndAttr
    FENameAndAttr       struct
        FENAA_attr          FileAttrs
        FENAA_name          FileLongName
    FENameAndAttr       ends

This structure is used with FileEnum.

**Library:** fileEnum.def

----------
#### FFA_stackFrame
    FFA_stackFrame      union
        FFA_float               FloatFloatToAsciiData
        FFA_dateTime            FloatFloatToDateTimeData
    FFA_stackFrame      end

**Library:** math.def

----------
#### FFCFeatures
    FFCFeatures     record
                                    :14
        FCF_FORMAT_LIST             :1
        FCF_DEFINE_FORMATS          :1
    FFCFeatures     end

**Library:** math.def

----------
#### FieldBGFormatType
    FieldBGFormatType       etype   word
        FBGFT_STANDARD_GSTRING              enum FieldBGFormatType
        ;Just a standard graphics string.
        FBGFT_BITMAP_SLICES                 enum FieldBGFormatType
        ;(Not currently supported)

**Library:** backgrnd.def

----------
#### FieldInfo
    FieldInfo       struct
        FI_nChars       word            ; Number of characters in the field
        FI_position     word            ; X position of field on line
        FI_width        word            ; Width of the field
        FI_tab          TabReference    ; Reference to a tab in the ruler
    FieldInfo       ends

**Library:** text.def

----------
#### FileAccess
    FileAccess      etype   byte, 0
        FA_READ_ONLY            enum FileAccess
        FA_WRITE_ONLY           enum FileAccess
        FA_READ_WRITE           enum FileAccess

**Library:** file.def

----------
#### FileAccessFlags
    FileAccessFlags         record
                        :1=0,           ; Must be 0.
        FAF_EXCLUDE     FileExclude:3,  ; What others may not do.
                        :2=0,           ; Must be 0.
        FAF_MODE        FileAccess:2,   ; What caller wants to do.
    FileAccessFlags         end

**Library:** file.def

----------
#### FileAddStandardPathFlags
    FileAddStandardPathFlags        record
                                :16
    FileAddStandardPathFlags        end

**Library:** 

----------
#### FileAttrs
    FileAttrs       record
        :1=0,
        FA_LINK     :1      ; File is a link
        FA_ARCHIVE  :1,     ; File requires backup (modified since FA_ARCHIVE
                            ; last cleared)
        FA_SUBDIR   :1,     ; File is actually a subdirectory
        FA_VOLUME   :1,     ; File is actually a volume label
        FA_SYSTEM   :1,     ; File is for the system (kernel, e.g.)
        FA_HIDDEN   :1,     ; File should not be seen by regular searches.
        FA_RDONLY   :1,     ; File may not be written
    FileAttrs       end

**Library:** file.def

----------
#### FileChangeBatchNotificationData
    FileChangeBatchNotificationData                 struct
        FCBND_end           nptr
        FCBND_items         label FileChangeBatchNotificationItem
    FileChangeBatchNotificationData                 ends

*FCBND_end* stores the ending offset of the array of 
**FileChangeBatchNotificationItem** structures.

**Library:** gcnlist.def

----------
#### FileChangeBatchNotificationItem
    FileChangeBatchNotificationItem                 struct
        FCBNI_type      FileChangeNotificationType
        FCBNI_disk      word
        FCBNI_id        FileID
        FCBNI_name      label FileLongName  ; Only present if required by FCBNI_type.
        FileChangeBatchNotificationItem             ends

**Library:** gcnlist.def

----------
#### FileChangeNotificationData
    FileChangeNotificationData              struct
        FCND_disk   word            ; handle for disk on which the change occurred
        FCND_id     FileID          ; 32-bit identifier for the directory in 
                                    ; which the change occurred, or for the file 
                                    ; to which the change occurred.
        FCND_name   FileLongName    ; For those notifications that require it, 
                                    ; the virtual name of the file or directory
                                    ; to which the change occurred.
    FileChangeNotificationData              ends

**Library:** gcnlist.def

----------
#### FileChangeNotificationType
    FileChangeNotificationType              etype word
        FCNT_CREATE             enum FileChangeNotificationType
            ; File or directory created. FCND_id is the id of the containing
            ; directory. FCND_name is the name of the new file or directory.

        FCNT_RENAME             enum FileChangeNotificationType
            ; File or directory renamed. FCND_id is the identifier for the file 
            ; or directory, and FCND_name is its new name.

        FCNT_OPEN               enum FileChangeNotificationType
            ; A file has been closed. FCND_id is the 32-bit identifier for the
            ; file. FCND_name is undefined and may not be present. This is
            ; generated only if someone has called 
            ; FileEnableOpenCloseNotification.

        FCNT_DELETE             enum FileChangeNotificationType
            ; File or directory deleted. FCND_id is the 32-bit identifier for 
            ; the file or directory that was deleted. FCND_name is undefined and
            ; may not be present.

        FCNT_CONTENTS           enum FileChangeNotificationType
            ; File contents changed. This is sent only when FileCommit or 
            ; FileClose is called for a file and the file has been modified. 
            ; FCND_id is the 32-bit identifier for the affected file. FCND_name 
            ; is undefined and may not be present.

        FCNT_ATTRIBUTES         enum FileChangeNotificationType
            ; File attributes changed. This is sent once all changes have been
            ; made during a given FileSetAttributes, FileSetHandleExtAttributes,
            ; or FileSetPathExtAttributes call. FCND_id is the 32-bit identifier
            ; for the affected file. FCND_name is undefined and may not be 
            ; present.

        FCNT_DISK_FORMAT        enum FileChangeNotificationType
            ; A disk has been formatted. FCND_id and FCND_name are undefined and
            ; may not be present.

        FCNT_CLOSE              enum FileChangeNotificationType
            ; A file has been closed. FCND_id is the 32-bit identifier for the
            ; file. FCND_name is undefined and may not be present. This is
            ; generated only if someone has called 
            ; FileEnableOpenCloseNotification.

        FCNT_BATCH              enum FileChangeNotificationType
            ; The block is a FileChangeBatchNotificationData block, holding
            ; multiple notifications. Notifications are collected into a batch
            ; when a thread calls FileBatchChangeNotifications. All the
            ; notifications are sent when it calls FileFlushChangeNotifications.
            ; Any application performing a substantial number of changes to
            ; the file system (e.g. deleting a directory tree) should tell the
            ; system to batch its notifications and flush them when it's all    ; done. This will reduce the number of handles required to alert all
            ; interested parties to the changes.

**Library:** gcnlist.def

----------
#### FileCreateFlags
    FileCreateFlags     record
        FCF_NATIVE                  :1
        FCF_NATIVE_WITH_EXT_ATTRS   :1
                                    :4
        FCF_MODE                    FileCreateMode:2
    FileCreateFlags     end

FCF_NATIVE  
Create file to be compatible with the file system on which it resides. This may 
mean that most extended attributes are not supported for the file, unless the 
file system itself supports them (which DOS file systems do not).

FCF_NATIVE_WITH_EXT_ATTRS  
Create file with a name compatible with the file system on which it resides, 
but support extended attributes. The driver may place restrictions on what 
sort of name may be used, and will return ERROR_INVALID_NAME if the 
name passed falls beyond the pale.

FCF_MODE  
How the file should be created.

**Library:** file.def

----------
#### FileCreateMode
    FileCreateMode      etype   byte, 0
        FILE_CREATE_TRUNCATE            enum FileCreateMode
        FILE_CREATE_NO_TRUNCATE         enum FileCreateMode
        FILE_CREATE_ONLY                enum FileCreateMode

**Library:** file.def

----------
#### FileDate
    FileDate        record
        FD_YEAR         :7,     ; year since 1980   
        FD_MONTH        :4,     ; month (1-12)
        FD_DAY          :5,     ; day of the month (1-31)
    FileDate        end

**Library:** file.def

----------
#### FileDateAndTime
    FileDateAndTime     struct
        FDAT_date           FileDate
        FDAT_time           FileTime
    FileDateAndTime     ends

**Library:** file.def
----------
#### FileEnumCallbackData
    FileEnumCallbackData            struct
        FECD_attrs      label FileExtAttrDesc
    FileEnumCallbackData            ends

*FECD_attrs* stores the array of extended-attribute descriptors for the current 
file. The end of the array is signaled by a **FileExtAttrDesc** with 
FEA_END_OF_LIST in its *FEAD_attr* field. All the attribute values lie in the 
same segment as the **FileEnumCallbackData**, so their 
*FEAD_value*.segment will be **ds** unless the file doesn't have that particular 
attribute, in which case *FEAD_value*.segment will be 0.

**Library:** fileEnum.def

----------
#### FileEnumParams
    FileEnumParams      struct
        FEP_searchFlags         FileEnumSearchFlags 0
        FEP_returnAttrs         fptr.FileExtAttrDesc 0
        FEP_returnSize          word 0
        FEP_matchAttrs          fptr.FileExtAttrDesc 0
        FEP_bufSize             word FE_BUFSIZE_UNLIMITED
        ;
        FE_BUFSIZE_UNLIMITED    equ 1   ; Value to pass in FEP_bufSize
                                        ; to place no limit on the
                                        ; number of files for which
                                        ; to return data.
        FEP_skipCount           word 0
        FEP_callback            fptr.far 0
        FEP_callbackAttrs       fptr.FileExtAttrDesc
        FEP_cbData1             dword 0
        FEP_cbData2             dword 0
        FEP_headerSize          word 0
        even
    FileEnumParams      ends

*FEP_searchFlags* stores the flags to control the **FileEnum** search operation.

**FEP_returnAttrs** stores the array of attributes that are to be returned from 
the **FileEnum** operation. The entries in the returned buffer can be of an 
arbitrary size; the size is controlled by the *FEP_returnSize* parameter. Each 
extended attribute returned for a file that matches is copied into the entry at 
an offset given by the *FEAD_value*.offset for the attribute. The number of 
bytes copied cannot exceed the value of *FEAD_size*.

If the segment is 0, the offset is of type **FileEnumStandardReturnType**, 
specifying the attributes to be returned for a standard structure (as defined 
later in this file). *FEP_returnSize* should still be either the size of the 
appropriate structure, or larger if that structure has been incorporated into 
a larger one of your own. The standard return type FESRT_COUNT_ONLY 
exists so you receive no information about the files that match, just their 
number (in **dx**). One of *FEP_bufSize* or *FEP_returnSize* should be 0 in this 
case.

The last entry in the array has FEA_END_OF_LIST as its *FEAD_attr*.

*FEP_returnSize* stores the size of each entry in the returned buffer.

*FEP_matchAttrs* stores the array of attributes that are to be matched by 
**FileEnum** itself. For attributes that are records (and hence a word or 
smaller), *FEAD_value*.offset holds the bits that must be set in the attribute, 
while *FEAD_value*.segment holds the bits that must not be set in the 
attribute's actual value. For all other attributes, *FEAD_value* is a pointer to 
the exact value to match. *FEAD_size* gives the size of that value.

The last entry in the array has FEA_END_OF_LIST as its *FEAD_attr*. If all the 
checks are to be performed by the callback, or if all files are desired, 
regardless of their attributes, *FEP_matchAttrs*.segment may be passed as 0. 
*FEP_matchAttrs*.offset may be anything in this case.

*FEP_bufSize* stores the number of structures that *FEP_buffer* can hold. This 
is used as the maximum number of files to find. The actual size of the buffer 
(in bytes) is determined by this and the *FEP_returnType*. If set to 0, the **dx** 
returned is a count of the matching files in the directory.

*FEP_skipCount* stores the number of matches to skip before storing matching 
entries in *FEP_buffer*. This can be used to make several passes through the 
files in a directory. Each pass will process the next *X* number of files in the 
directory: 

**FileEnum**(skipCount=0, FEP_bufSize=20)  
process(FEP_buffer)  
**FileEnum**(skipCount=20, FEP_bufSize=20)  
process(FEP_buffer)  
**FileEnum**(skipCount=40, FEP_bufSize=20)  
process(FEP_buffer)  
**FileEnum**(skipCount=60, FEP_bufSize=20)  
process(FEP_buffer)

This means that a buffer that only holds 20 files may be used as opposed to a 
buffer of unknown size which would otherwise be needed to hold return 
structures for all files in the directory.

Skip count optimization - if the FESF_REAL_SKIP bit is set, then this is the 
actual number of files to skip, matching or not. If FESF_REAL_SKIP is clear, 
*FEP_skipCount* is the number of matching files to skip. The real skip count is 
faster because the match condition does not need to be checked.

With FESF_REAL_SKIP set: When **FileEnum** returns after filling in 
*FEP_bufSize* number of matching entries, **di** will be updated to the real 
number of files passed through in order to get those *FEP_bufSize* files.

Starting with **di** at 0, **FileEnum** will increment **di** once for each file 
checked. When **FileEnum** returns, save **di** for the next time **FileEnum** is 
called.

*FEP_callback* stores the address of the callback routine to determine if the 
file should be accepted by **FileEnum**. The callback is performed after all 
regularly specified tests have accepted the file. Therefore, this callback routine 
is the last step when checking acceptance of the file.

**Callback Routine Specifications:**  
**Passed:**  
 - ds   = segment of **FileEnumCallbackData**  
 - bp   = inherited stack frame, which must be 
passed to any **FileEnum** helper routines the 
callback calls.

The callback routine can look at the **FileEnumStructure** passed by using:

    FooCallback proc far params:FileEnumParams 
    enter inherit far

**Return:**  
 - carry clear to accept file  
 - carry set to reject file

Callback routine should destroy no registers and should not change passed 
structures. Only relevant if mask FESF_CALLBACK is set.

If FESF_CALLBACK is set and *FEP_callback*.segment is 0, *FEP_callback*.offset 
is a **FileEnumStandardCallback**. *FEP_callbackAttrs* is ignored in this 
case, as the system knows what extra attributes are required by each 
standard callback. See the description of each FESC_* constant to find what 
should be passed in *FEP_cbData1* and *FEP_cbData2*.

*FEP_callbackAttrs* specifies an array of attributes the callback routine will 
need to examine if segment is non-zero, and FESF_CALLBACK is set in 
*FEP_searchFlags*. **FileEnum** will always pass the callback all attributes 
given in either the *FEP_returnAttrs* or *FEP_matchAttrs* array. This array is 
for attributes for which you can't give an exact value (thus they can't be in 
*FEP_matchAttrs*) and of which you don't actually need to make a record (thus 
they can't be in *FEP_returnAttrs*).

The last entry in the array has FEA_END_OF_LIST as its *FEAD_attr*. If no 
additional attributes are required when FESF_CALLBACK is set, 
*FEP_callbackAttrs*.segment must be zero.

*FEP_cbData1* and *FEP_cbData2* allow the caller of **FileEnum** to pass data to 
the callback routine.

*FEP_headerSize* stores the amount of space to leave at the start of the return 
block if FESF_LEAVE_HEADER set. 

**Library:** fileEnum.def

----------
#### FileEnumSearchFlags
    FileEnumSearchFlags             record
        FESF_DIRS               :1  ; accept directories
        FESF_NON_GEOS           :1  ; accept non-GEOS files
        FESF_GEOS_EXECS         :1  ; accept GEOS executables
        FESF_GEOS_NON_EXECS     :1  ; accept GEOS non-executables (data files)
        FESF_REAL_SKIP          :1  ; use FEP_skipCount is real skip count
                                    ; (see FileEnum for explanation)
        FESF_CALLBACK           :1  ; use FEP_callback field
        FESF_LOCK_CB_DATA       :1  ; for use in FileEnumPtr only; if set,
                                    ; FEP_cbData1 and FEP_cbData2 are assumed
                                    ; to be far pointers to movable or fixed
                                    ; memory that must be locked before
                                    ; FileEnum is called.
        FESF_LEAVE_HEADER       :1  ; if set, then FEP_headerSize indicates
                                    ; number of bytes at the beginning of the
                                    ; return block that should be left 0 by
                                    ; FileEnum, to form a header to be filled
                                    ; in by the caller.
    FileEnumSearchFlags             end

**Library:** fileEnum.def

----------
#### FileEnumStandardCallback
    FileEnumStandardCallback            etype       word, 0
        FESC_WILDCARD           enum    FileEnumStandardCallback
            ;FEP_cbData1 is a far pointer to a null-terminated string 
            ;containing a virtual filename, with the special characters * and ?
            ;interpreted as meaning 0-or-more-of-any-character and 
            ;any-character, respectively.
            ;
            ;Note that the match occurs in the virtual namespace, so "*.*" will
            ;not match all files, as it will in standard DOS, but rather all
            ;files that have a . in their virtual name.
            ;
            ;FEP_cbData2.low should be non-zero to perform the match in a
            ;case-insensitive fashion, or zero to be case-sensitive.

**Library:** fileEnum.def

----------
#### FileEnumStandardReturnType
    FileEnumStandardReturnType              etype word, 0
        FESRT_COUNT_ONLY                enum FileEnumStandardReturnType
        FESRT_DOS_INFO                  enum FileEnumStandardReturnType
        FESRT_NAME                      enum FileEnumStandardReturnType
        FESRT_NAME_AND_ATTR             enum FileEnumStandardReturnType

**Library:** fileEnum.def

----------
#### FileError
    FileError       etype word
        ERROR_UNSUPPORTED_FUNCTION      enum FileError, 1   ;MS-DOS error
        ERROR_FILE_NOT_FOUND            enum FileError, 2   ;MS-DOS error
        ERROR_PATH_NOT_FOUND            enum FileError, 3   ;MS-DOS error
        ERROR_TOO_MANY_OPEN_FILES       enum FileError, 4   ;MS-DOS error
        ERROR_ACCESS_DENIED             enum FileError, 5   ;MS-DOS error
        ERROR_INSUFFICIENT_MEMORY       enum FileError, 8   ;MS-DOS & FileEnum error
        ERROR_INVALID_DRIVE             enum FileError, 15  ;MS-DOS error
        ERROR_IS_CURRENT_DIRECTORY      enum FileError, 16  ;MS-DOS error
        ERROR_DIFFERENT_DEVICE          enum FileError, 17  ;MS-DOS error
        ERROR_NO_MORE_FILES             enum FileError, 18  ;MS-DOS error
        ERROR_WRITE_PROTECTED           enum FileError, 19  ;MS-DOS critical error
        ERROR_UNKNOWN_VOLUME            enum FileError, 20  ;MS-DOS critical error
        ERROR_DRIVE_NOT_READY           enum FileError, 21  ;MS-DOS critical error
        ERROR_CRC_ERROR                 enum FileError, 23  ;MS-DOS critical error
        ERROR_SEEK_ERROR                enum FileError, 25  ;MS-DOS critical error
        ERROR_UNKNOWN_MEDIA             enum FileError, 26  ;MS-DOS critical error
        ERROR_SECTOR_NOT_FOUND          enum FileError, 27  ;MS-DOS critical error
        ERROR_WRITE_FAULT               enum FileError, 29  ;MS-DOS critical error
        ERROR_READ_FAULT                enum FileError, 30  ;MS-DOS critical error
        ERROR_GENERAL_FAILURE           enum FileError, 31  ;MS-DOS critical error
        ERROR_SHARING_VIOLATION         enum FileError, 32  ;
        ERROR_ALREADY_LOCKED            enum FileError, 33  ;'share.exe' error
        ERROR_SHARING_OVERFLOW          enum FileError, 36  ;'share.exe' error
        ERROR_NETWORK_CONNECTION_BROKEN enum FileError, 55
        ERROR_NETWORK_ACCESS_DENIED     enum FileError, 65
        ERROR_NETWORK_NOT_LOGGED_IN     enum FileError, 78
        ERROR_SHORT_READ_WRITE          enum FileError, 128 ;PC GEOS error
        ERROR_INVALID_NAME              enum FileError, 129 ;PC GEOS error
        ERROR_FILE_EXISTS               enum FileError, 130
        ERROR_DOS_EXEC_IN_PROGRESS      enum FileError, 131; DosExec
        ERROR_FILE_IN_USE               enum FileError, 132
        ERROR_ARGS_TOO_LONG             enum FileError, 133 ;DosExec
        ERROR_DISK_UNAVAILABLE          enum FileError, 134 ;Validation of disk in 
                                            ;drive aborted by user.
        ERROR_DISK_STALE                enum FileError, 135 ;Drive disk was on has been
                                            ;removed.
        ERROR_FILE_FORMAT_MISMATCH      enum FileError, 136 ;Attempted to create a file
                                            ;with FILE_CREATE_TRUNCATE or
                                            ;FILE_CREATE_NO_TRUNCATE and its 
                                            ;current state doesn't match that 
                                            ;desired by the FCF_NATIVE flag.
        ERROR_CANNOT_MAP_NAME           enum FileError, 137 ;file system driver was 
                                            ;unable to map the virtual 32-char
                                            ;name to a suitable name appropriate to 
                                            ;the file system.
        ERROR_DIRECTORY_NOT_EMPTY       enum FileError, 138 ;Attempted to delete a 
                                            ;directory that still contained files.
        ERROR_ATTR_NOT_SUPPORTED        enum FileError, 139 ;Requested an extended 
                                            ;attribute that is not supported by the
                                            ;file system or the file.
        ERROR_ATTR_NOT_FOUND            enum FileError, 140 ;Requested an extended 
                                            ;attribute that is not present for the
                                            ;file.
        ERROR_ATTR_SIZE_MISMATCH        enum FileError, 141 ;Requested an attribute 
                                            ;without providing the correct amount of
                                            ;space/data to get/set it.
        ERROR_ATTR_CANNOT_BE_SET        enum FileError, 142 ;Attempted to set an 
                                            ; extended attribute that cannot be set:
                                            ;       FEA_SIZE
                                            ;       FEA_NAME
                                            ;       FEA_DOS_NAME
                                            ;       FEA_GEODE_ATTRS
                                            ;       FEA_PATH_INFO
                                            ;       FEA_FILE_ID
        ERROR_CANNOT_MOVE_DIRECTORY     enum FileError, 143;file system doesn't support
                                            ; moving of directories in
                                            ; FileMove, and PC/GEOS doesn't
                                            ; provide the functionality
                                            ; itself.
        ERROR_PATH_TOO_LONG             enum FileError, 144 ;Attempted to create a 
                                            ;directory that would be unreachable,
                                            ; owing to path-length
                                            ; restrictions of the file system
        ERROR_ARGS_INVALID              enum FileError, 145 ;DosExec: argument string
                                            ; contained a character that
                                            ; could not be mapped to the
                                            ; current DOS character set.
        ERROR_CANNOT_FIND_COMMAND_INTERPRETER enum FileError, 146
                                            ;DosExec: program to run is a
                                            ; batch file, but system was
                                            ; unable to locate the command
                                            ; interpreter (COMMAND.COM) to
                                            ; run the command.
        ERROR_NO_TASK_DRIVER_LOADED     enum FileError, 147 
                                            ;DosExec: cannot run a DOS program as 
                                            ; no task-switching driver was loaded.
        ERROR_LINK_ENCOUNTERED          enum FileError, 148
                                            ; A link was encountered and needs to 
                                            ; be traversed
        ERROR_NOT_A_LINK                enum FileError, 149 
                                            ; A link function was called on a file 
                                            ; that's not a link.
        ERROR_TOO_MANY_LINKS            enum FileError, 150 
                                            ; A path contains too many links. Most 
                                            ; likely, one of the elements of the path
                                            ; is a link to itself.

**Library:** file.def

----------
#### FileExclude
    FileExclude     etype   byte, 0
        FE_COMPAT           enum FileExclude
        FE_EXCLUSIVE        enum FileExclude
        FE_DENY_WRITE       enum FileExclude
        FE_DENY_READ        enum FileExclude
        FE_NONE             enum FileExclude

**Library:** file.def

----------
#### FileExtAttrDesc
    FileExtAttrDesc     struct
        FEAD_attr           FileExtendedAttribute
        FEAD_value          fptr
        FEAD_size           word
        FEAD_name           fptr.char
    FileExtAttrDesc     ends

This structure stores a description of extended attributes that should be 
changed to reflect new values.

*FEAD_attr* stores the **FileExtendedAttribute** that is to be altered (or 
FEA_CUSTOM to alter a custom attribute). *FEAD_value* stores the pointer to 
a buffer containing the new value.

*FEAD_size* stores the size of that buffer.

*FEAD_name* stores the pointer to a null-terminated ASCII name of an 
attribute if the attribute is a custom one (FEA_CUSTOM).

**FileEnum** can be passed arrays of **FileExtAttrDesc** structures. In this 
case, the number of elements in the array is not passed; the last element 
should have its *FEAD_attr* field set to FEA_END_OF_LIST.

**Library:** file.def

----------
#### FileExtendedAttribute
    FileExtendedAttribute           etype word, 0
        FEA_MODIFICATION enum FileExtendedAttribute ; FileDateAndTime
        FEA_FILE_ATTR   enum FileExtendedAttribute  ; FileAttrs
        FEA_SIZE        enum FileExtendedAttribute  ; dword
        FEA_FILE_TYPE   enum FileExtendedAttribute  ; GeosFileType
        FEA_FLAGS       enum FileExtendedAttribute  ; GeosFileHeaderFlags
        FEA_RELEASE     enum FileExtendedAttribute  ; ReleaseNumber
        FEA_PROTOCOL    enum FileExtendedAttribute  ; ProtocolNumber
        FEA_TOKEN       enum FileExtendedAttribute  ; GeodeToken
        FEA_CREATOR     enum FileExtendedAttribute  ; GeodeToken
        FEA_USER_NOTES  enum FileExtendedAttribute  ; char array
                                                    ; FileUserNotes
        FEA_NOTICE      enum FileExtendedAttribute  ; char array
                                                    ; FileCopyrightNotice
        FEA_CREATION    enum FileExtendedAttribute  ; FileDateAndTime
        FEA_PASSWORD    enum FileExtendedAttribute  ; char array
                                                    ; FilePassword
        FEA_CUSTOM      enum FileExtendedAttribute  ; ?
        FEA_NAME        enum FileExtendedAttribute  ; char array
                                                    ; (FileLongName)
        FEA_GEODE_ATTR  enum FileExtendedAttribute  ; GeodeAttrs. a hack
                                                    ; for FileEnum...
        FEA_PATH_INFO   enum FileExtendedAttribute  ; DirPathInfo. a hack
                                                    ; for FileEnum...
        FEA_FILE_ID     enum FileExtendedAttribute  ; 32-bit ID of
                                                    ; file
        FEA_DESKTOP_INFO enum FileExtendedAttribute ; FileDesktopInfo
        FEA_DRIVE_STATUS enum FileExtendedAttribute ; DriveExtendedStatus
        FEA_DISK        enum FileExtendedAttribute  ; Disk handle
        ;
        ; these next are supported only by some file systems and are intended for
        ; specialized use (e.g. a desktop program) not for most applications.
        ;
        FEA_DOS_NAME    enum FileExtendedAttribute  ; actual DOS name of
                                                    ;the file, if it's on 
                                                    ;a DOS file system. 
                                                    ;8.3. name in DOS 
                                                    ;character set
        FEA_OWNER       enum FileExtendedAttribute  ; null-terminated name
                                                    ; of owner of the file.
                                                    ; FileOwnerName
        FEA_RIGHTS      enum FileExtendedAttribute  ; null-terminated
                                                    ; description of access
                                                    ; rights to the file.
                                                    ; FileAccessRights
        FEA_LAST_VALID  equ FileExtendedAttribute-1
        FEA_MULTIPLE    enum FileExtendedAttribute,-2 ; Special value for
                                                    ; FileGetExtAttr and
                                                    ; FileSetExtAttr to
                                                    ; get/set multiple
                                                    ; attributes for a
                                                    ; file.
        FEA_END_OF_LIST enum FileExtendedAttribute,-1; Marker for the last
                                                    ; entry in an array of
                                                    ; FileExtAttrDesc
                                                    ; structures

**Library:** file.def

----------
#### FileOpenAndReadFlags
    FileOpenAndReadFlags            record
        ; These three flags are processed in order:

        FOARF_ADD_CRLF:1
        ; Append a CR/LF to the buffer, unless the buffer already ends
        ; with a CR/LF.

        FOARF_ADD_EOF:1
        ; Append an MSDOS_TEXT_FILE_EOF to the buffer.

        FOARF_NULL_TERMINATE:1
        ; null-terminate the buffer.

        :6
        FOARF_ACCESS FileAccessFlags:7
    FileOpenAndReadFlags            end

**Library:** file.def

----------
#### FilePathID
    FilePathID      struct
        FPID_disk           word        ; disk handle
        FPID_id             FileID      ; ID for path on that disk.
    FilePathID      ends

These structures act as elements of arrays returned by 
**FileGetCurrentPathIDs**.

**Library:** file.def

----------
#### FilePosMode
    FilePosMode     etype   byte, 0
        FILE_POS_START              enum FilePosMode
        FILE_POS_RELATIVE           enum FilePosMode
        FILE_POS_END                enum FilePosMode

**Library:** file.def

----------
#### FileSelectorAttrs
    FileSelectorAttrs           record
        FSA_ALLOW_CHANGE_DIRS           :1
                    :1
        FSA_SHOW_FIXED_DISKS_ONLY       :1
        FSA_SHOW_FILES_DISABLED         :1
        FSA_HAS_CLOSE_DIR_BUTTON        :1
        FSA_HAS_OPEN_DIR_BUTTON         :1
        FSA_HAS_DOCUMENT_BUTTON         :1
        FSA_HAS_CHANGE_DIRECTORY_LIST   :1
        FSA_HAS_CHANGE_DRIVE_LIST       :1
        FSA_HAS_FILE_LIST               :1
        FSA_USE_VIRTUAL_ROOT            :1
                                        :5
    FileSelectorAttrs           end

FSA_ALLOW_CHANGE_DIRS  
Allows changing to different directories. If not set, directories 
are not opened automatically when the user double-clicks on 
them. It is up to the application to send 
MSG_FILE_SELECTOR_OPEN_ENTRY to the GenFileSelector.

FSA_SHOW_FIXED_DISKS_ONLY  
Show only fixed disks in the volume existing.

FSA_SHOW_FILES_DISABLED  
When showing a file, don't allow the user to select it; useful for 
Save As operations.

FSA_HAS_CLOSE_DIR_BUTTON  
Set if the corresponding gadget.

FSA_HAS_OPEN_DIR_BUTTON  
Appear in the file selector

FSA_HAS_DOCUMENT_BUTTON

FSA_HAS_CHANGE_DIRECTORY_LIST

FSA_HAS_CHANGE_DRIVE_LIST

FSA_HAS_FILE_LIST

FSA_USE_VIRTUAL_ROOT  
Set if information in 
ATTR_GEN_FILE_SELECTOR_VIRTUAL_ROOT should be used 
(allows turning on and off 'virtual root' feature without 
changing variable data).

**Library:** Objects/gFSelC.def

----------
#### FileSelectorFileCriteria
    FileSelectorFileCriteria            record
        ;
        ; Types of files to include in the listing
        ; 
        FSFC_DIRS                   :1      ; include directories
        FSFC_NON_GEOS_FILES         :1      ; include non-GEOS files
        FSFC_GEOS_EXECUTABLES       :1      ; include GEOS executables
        FSFC_GEOS_NON_EXECUTABLES   :1      ; include GEOS non-executables
        ;
        ; for files and (if FSFC_USE_MASK_FOR_DIRS is set) directories
        ;
        FSFC_MASK_CASE_SENSITIVE    :1
        ;
        ; for all files (FSFC_NON_GEOS_FILES and/or FSFC_GEOS_EXECUTABLESS
        ;   and/or FSFC_GEOS_NON_EXECUTABLES)
        ;
        FSFC_FILE_FILTER            :1
        FSFC_FILTER_IS_C            :1
        ;
        ; for GEOS files (FSFC_GEOS_EXECS_FILES and/or
        ;   FSFC_GEOS_NON_EXECUTABLES)
        ;
        FSFC_TOKEN_NO_ID            :1
        ;
        ; for directories (FSFC_DIRS)
        ;
        FSFC_USE_MASK_FOR_DIRS      :1
                                    :7
    FileSelectorFileCriteria            end

This record defines the file selection criteria of a **GenFileSelector**. This 
information is stored in the file selector's *GFSI_fileCriteria* instance field.

FSFC_MASK_CASE_INSENSITIVE  
Match files against the mask in a case-insensitive manner.

FSFC_FILE_FILTER  
Use the filter routine in addition to evaluating each file accepted by other 
selection criteria.

FSFC_FILTER_IS_C  
The filter routine returned by 
MSG_GEN_FILE_SELECTOR_GET_FILTER_ROUTINE is written in C and 
obeys the Pascal calling convention.

FSFC_TOKEN_NO_ID  
Ignore manufacturer ID when comparing tokens (for FSFC_TOKEN_MATCH 
and FSFC_CREATOR_MATCH).

FSFC_USE_MASK_FOR_DIRS  
Also use ATTR_GEN_FILE_SELECTOR_NAME_MASK attribute for directories. 
(This ATTR is normally applied only to files.)

**Library:** Objects/gFSelC.def

----------
#### FileTime
    FileTime            record
        FT_HOUR     :5,     ; hour (24-hour clock)
        FT_MIN      :6,     ; minute (0-59)
        FT_2SEC     :5,     ; 2-second (0-29 giving 0-58 seconds, even 
                            ;seconds only)
    FileTime            end

**Library:** file.def

----------
#### FindNoteHeader
    FindNoteHeader      struct
        FNH_count   word            ; The number of matching notes.
        FNH_data    label dword
    FindNoteHeader      ends

**Library:** pen.def

----------
#### FloatAsciiToFloatFlags
    FloatAsciiToFloatFlags          record
                                :6
        FAF_PUSH_RESULT         :1
        FAF_STORE_NUMBER        :1
    FloatAsciiToFloatFlags          end

This record is used by the **FloatFloatToAscii** routine. This routine converts 
ASCII text into a floating point (FP) number.

FAF_PUSH_RESULT  
This flag specifies that the resulting FP number should be pushed onto the 
FP stack.

FAF_STORE_NUMBER  
This flag specifies that the resulting FP number should be stored in the buffer 
passed in **FloatFloatToAscii**.

**Library:** math.def

----------
#### FloatCtrlInfoStruc
    FloatCtrlInfoStruc          struc
        ;
        ; passed values
        ;
        FCIS_listEntryNum           word
        FCIS_fmtToken               word
        FCIS_listOD                 dword
        FCIS_fmtArrayHan            word
        FCIS_fmtArraySeg            word
        ;
        ; returned values
        ;
    FCIS_fmtParamsHan               word
    FloatCtrlInfoStruc          ends

*FCIS_listEntryNum* stores the zero-based position of the format entry to 
retrieve in the Float Format controller.

*FCIS_fmtToken* stores the token of the format entry to retrieve in the Float 
Format controller.

*FCIS_listOD* stores the optr of the dynamic list within the Float Format 
controller. This list stores the format entry names.

*FCIS_fmtArrayHan* stores the VM file handle of the array storing 
user-defined formats. This handle must be passed to all routines that operate 
on formats even if they do not directly access this format array (i.e. even if 
there are no user-defined formats).

*FCIS_fmtArraySeg* stores the VM block handle of the array storing 
user-defined formats. This segment must be passed to all routines that 
operate on formats even if they do not directly access this format array.

*FCIS_fmtParamsHan* stores the handle to a **FormatParams** structure 
returned by the routine.

**Library:** math.def

----------
#### FloatErrorType
    FloatErrorType      etype   byte, FLOAT_ERROR_CODES_ENUM_START, 1
        FLOAT_POS_INFINITY              enum FloatErrorType
        FLOAT_NEG_INFINITY              enum FloatErrorType
        FLOAT_GEN_ERR                   enum FloatErrorType

**Library:** math.def

----------
#### FloatExponent
    FloatExponent           record
        FE_SIGN         :1      ; set if number is negative
        FE_EXPONENT     :15     ; the exponent is biased by 3fffh
    FloatExponent           end

This record defines the high word of a GEOS 80 bit FP number. This high 
word stores the sign (FE_SIGN) and a 15 bit exponent (FE_EXPONENT).

You can check if the FP number has overflowed or underflowed by checking 
FE_EXPONENT against FP_NAN.

**Library:** math.def

----------
#### FloatFloatToAsciiData
    FloatFloatToAsciiData           struct
        ;
        ; Fields for caller to set up. All fields must be initialized.
        ;
        FFA_params              FloatFloatToAsciiParams
        ;
        ; Possibly useful information returned by FloatFloatToAscii.
        ;
        FFA_startNumber         word
        FFA_decimalPoint        word
        FFA_endNumber           word
        FFA_numChars            word
        FFA_startExponent       word
        FFA_bufSize             word        ;internal use only
        FFA_saveDI              word        ;internal use only
        FFA_numSign             word        ;internal use only
        FFA_startSigCount       byte        ;internal use only
        FFA_sigCount            byte        ;internal use only
        FFA_noMoreSigInfo       byte        ;internal use only
        FFA_startDecCount       byte        ;internal use only
        FFA_decCount            byte        ;internal use only
        FFA_decExponent         word        ;internal use only
        FFA_curExponent         word        ;internal use only
        FFA_useCommas           byte        ;internal use only
        FFA_charsToComma        byte        ;internal use only
        FFA_commaChar           char        ;internal use only
        FFA_decimalChar         char        ;internal use only
    FloatFloatToAsciiData           ends

This structure contains the **FloatFloatToAsciiParams** and some fields for 
internal use. This structure exists as a member of the *FFA_stackFrame* 
union.

*FFA_params* must be set up by the caller. All fields in that structure must be 
initialized.

*FFA_startNumber* returns the offset to the start of numeric characters. (This 
field is set by **FloatFloatToAscii**::**FloatDoPreNumeric**.)

*FFA_decimalPoint* returns the offset to the decimal point (0 if there is no 
decimal point). (This field is set by 
**FloatFloatToAscii**::**StuffDecimalPoint**.)

*FFA_endNumber* stores the offset to the end of numeric characters. (This 
field is set by **FloatFloatToAscii**:**FloatDoPostNumeric**.)

*FFA_numChars* stores the total number of characters in the ASCII string 
excluding the null terminator, or 0 if there was an error. (This field is set by 
**FloatFloatToAscii** in two locations.)

*FFA_startExponent* stores the offset to the "E" character in an exponentiated 
numeric value, or 0 if there is no exponent. Applications can check this to see 
if the exponent format was used.

**Library:** math.def

----------
#### FloatFloatToAsciiFormatFlags
    FloatFloatToAsciiFormatFlags                record
        FFAF_FLOAT_RESERVED                 :1
        FFAF_FROM_ADDR                      :1
                                            :4
        FFAF_DONT_USE_SCIENTIFIC            :1
        ;boolean bits, phrased so that a 0 will give the default
        FFAF_SCIENTIFIC                     :1
        FFAF_PERCENT                        :1
        FFAF_USE_COMMAS                     :1
        FFAF_NO_TRAIL_ZEROS                 :1
        FFAF_NO_LEAD_ZERO                   :1
        FFAF_HEADER_PRESENT                 :1
        FFAF_TRAILER_PRESENT                :1
        FFAF_SIGN_CHAR_TO_FOLLOW_HEADER     :1
        FFAF_SIGN_CHAR_TO_PRECEDE_TRAILER   :1
    FloatFloatToAsciiFormatFlags                end

FFAF_FLOAT_RESERVED  
This flag must be 0 to perform a float-to-ASCII operation (a 1 indicates a 
time-date operation).

FFAF_FROM_ADDR  
This flag is set if the number should be taken from a passed address rather 
than the top of the FP stack.

FFAF_DONT_USE_SCIENTIFIC  
This flag informs **FloatFloatToAscii** to format the number as a fixed point 
number by padding the number with zeros as necessary. The routine will 
force scientific anyway if the resulting string exceeds some large limit.

FFAF_SCIENTIFIC  
Set if the FP number should always be formatted in scientific notation.

FFAF_PERCENT  
Set if the FP number should be formatted as a percentage 

FFAF_USE_COMMAS
Set if the format should use commas to demarcate the thousands separators.

FFAF_NO_TRAIL_ZEROS  
Set if the format should use trailing zeros to pad the FP number.

FFAF_NO_LEAD_ZERO  
Set if a zero should precede the decimal point if the FP number is between -1 
and 1.

FFAF_HEADER_PRESENT  
This flag indicates that the format includes a header. Setting this flag speeds 
formatting.

FFAF_TRAILER_PRESENT  
This flag indicates that the format includes a trailer. Setting this flag speeds 
formatting.

FFAF_SIGN_CHAR_TO_FOLLOW_HEADER  
This flag indicates the position of the sign character(s).

FFAF_SIGN_CHAR_TO_PRECEDE_TRAILER  
This flag indicates the position of the sign character(s).

**Library:** math.def

----------
#### FloatFloatToAsciiParams
    FloatFloatToAsciiParams         struct
        ;
        ;Fields for caller to set up. All fields must be initialized.
        ;
        formatFlags     FloatFloatToAsciiFormatFlags
        decimalOffset   byte
        totalDigits     byte
        decimalLimit    byte
        preNegative     char SIGN_STR_LEN+1 dup (?)
        postNegative        char SIGN_STR_LEN+1 dup (?)
        prePositive     char SIGN_STR_LEN+1 dup (?)
        postPositive        char SIGN_STR_LEN+1 dup (?)
        ;
        ; HEADER AND TRAILER FOLLOW
        ; If these aren't present then only the bytes above need be stored
        ; per format
        ;
        header          char PAD_STR_LEN+1 dup (?)
        trailer         char PAD_STR_LEN+1 dup (?)
        align           word
    FloatFloatToAsciiParams         ends

*formatFlags* stores a record of Boolean bits specifying how the caller wants 
the string to look.

*decimalOffset* stores the number of decimal places that the caller want the 
decimal point to be offset. E.g. Caller may want offset of -6 to display 
numbers in terms of "millions."

DECIMAL_PRECISION <= *decimalOffset* <= DECIMAL_PRECISION.

*totalDigits* stores the maximum number of digits for the number to contain 
(integer + decimal portions). The ASCII string is truncated if length(string) > 
number. 

Generally,  
*totalDigits* <= DECIMAL_PRECISION if 
FFAF_DONT_USE_SCIENTIFIC is not used.  
*totalDigits* <= MAX_DIGITS_FOR_HUGE_NUMBERS if 
FFAF_DONT_USE_SCIENTIFIC is used.

By the way, a significant digit is a decimal digit derived from the floating 
point number's mantissa and it may precede or follow a decimal point. The 
IEEE format is only capable of DECIMAL_PRECISION number of significant 
digits. If the totalDigits is greater then DECIMAL_PRECISION, the excess 
digits will be 0.

*decimalLimit* stores the maximum number of decimal digits. The number 
will be rounded to meet this limit. E.g. 345.678 with a *decimalLimit* of 2 will 
return 345.68 in fixed format and 3.46E+2 in scientific format.

0 <= *decimalLimit* <= DECIMAL_PRECISION.

*preNegative* stores the character(s) used to precede a negative number. The 
string is expected to be null terminated. E.g. for parenthesized negatives 
*preNegative* may be '('; for arithmetic negatives *preNegative* may be '-'. Set 
*preNegative* to 0 if no character is desired.

(Note: the +1 in "SIGN_STR_LEN+1" is for the null terminator.)

*postNegative* stores the character(s) used to terminate a negative number. 
The string is expected to be null terminated. E.g. for parenthesized 
negatives, *postNegative* may be ')'. Set *postNegative* to 0 if no character is 
desired.

(Note: the +1 in "SIGN_STR_LEN+1" is for the null terminator.)

prePositive stores the character(s) used to precede a positive number. The 
string is expected to be null terminated. E.g. for arithmetic positives 
prePositive may be '+'. Set prePositive to 0 if no character is desired.

(Note: the +1 in "SIGN_STR_LEN+1" is for the null terminator.)

*postPositive* stores the character(s) used to terminate a positive number. The 
string is expected to be null terminated. Set *postPositive* to 0 if no character 
is desired.

(Note: the +1 in "SIGN_STR_LEN+1" is for the null terminator.)

*header* stores the characters that should precede the number. The string is 
expected to be null terminated. Whether or not this string follows or precedes 
the sign is determined by FFAF_SIGN_CHAR_TO_FOLLOW_HEADER.

(Note: the +1 in "PAD_STR_LEN+1" is for the null terminator.)

*trailer* stores the characters that should follow the number. The string is 
expected to be null terminated. Whether or not this string follows or precedes 
the sign is determined by FFAF_SIGN_CHAR_TO_PRECEDE_TRAILER.

(Note: the +1 in "PAD_STR_LEN+1" is for the null terminator.)

**Library:** math.def

----------
#### FloatFloatToAsciiParams_Union
    FloatFloatToAsciiParams         union
        FFAP_FLOAT          FloatFloatToAsciiParams
        FFAP_DATE_TIME      FloatFloatToDateTimeParams
    FloatFloatToAsciiParams         end

This union is used by **FloatFloatToAscii** to determine if the FP number 
should be converted into ASCII text or into a date-time.

**Library:** math.def

----------
#### FloatFloatToDateTimeData
    FloatFloatToDateTimeData            struct
        FFA_dateTimeParams          FloatFloatToDateTimeParams
    FloatFloatToDateTimeData            ends

**FloatFloatToDateTimeData** contains the 
**FloatFloatToDateTimeParams** and (possibly) fields for internal use in the 
future. This structure exists as a member of the *FFA_stackFrame* union.

**Library:** math.def

----------
#### FloatFloatToDateTimeFlags
    FloatFloatToDateTimeFlags               record
        ; these first 2 bits must not move
        FFDT_DATE_TIME_OP           :1
        FFDT_FROM_ADDR              :1
        FFDT_FORMAT                 :14
    FloatFloatToDateTimeFlags               end

FFDT_DATE_TIME_OP  
This flag must set to indicate to **FloatFloatToAscii** that a date operation is 
desired.

FFDT_FROM_ADDR  
This flag is set if the FP number to convert should be taken from a passed 
address rather than the top of the FP stack.

FFDT_FORMAT stores the **DateTimeFormat** format to use.

**Library:** math.def

----------
#### FloatFloatToDateTimeParams
    FloatFloatToDateTimeParams              struct
        FFA_dateTimeFlags           FloatFloatToDateTimeFlags
        FFA_year                    word
        FFA_month                   byte
        FFA_day                     byte
        FFA_weekday                 byte
        FFA_hours                   byte
        FFA_minutes                 byte
        FFA_seconds                 byte
    FloatFloatToDateTimeParams              ends

The **FloatFloatToDateTimeParams** portion of 
**FloatFloatToDateTimeData** needs to be initialized before the call to 
**FloatFloatToAscii**.

The **FloatFloatToDateTimeParams** is part of the 
**FloatFloatToDateTimeData** structure which in turn is a member of the 
*FFA_stackFrame* union.

**Library:** math.def

----------
#### FloatFormatErrors
    FloatFormatErrors       etype   byte, 0
        FLOAT_FORMAT_NO_ERROR                   enum FloatFormatErrors
        FLOAT_FORMAT_TOO_MANY_FORMATS           enum FloatFormatErrors
        FLOAT_FORMAT_CANNOT_ALLOC               enum FloatFormatErrors
        ;
        ; picked with FORMAT_ID_PREDEF in mind 
        ;
        FLOAT_FORMAT_FORMAT_NAME_NOT_FOUND      = 7fffh
        FLOAT_FORMAT_PARAMS_MATCH               = TRUE
        FLOAT_FORMAT_PARAMS_DONT_MATCH          = FALSE

**Library:** math.def

----------
#### FloatNum
    FloatNum        struct
        F_mantissa_wd0      word                ; offset 0
        F_mantissa_wd1      word                ; offset 2
        F_mantissa_wd2      word                ; offset 4
        F_mantissa_wd3      word                ; offset 6
        F_exponent          FloatExponent <>    ; offset 8
    FloatNum        ends

This structure defines a GEOS 80 bit floating point number.

**Library:** math.def

----------
#### FloatStackType
    FloatStackType      etype   byte, 0
        FLOAT_STACK_GROW                enum FloatStackType
        FLOAT_STACK_WRAP                enum FloatStackType
        FLOAT_STACK_ERROR               enum FloatStackType

        FLOAT_STACK_DEFAULT_TYPE        equ FLOAT_STACK_GROW

This value defines the type of FP stack that should be used by the current 
thread.

FLOAT_STACK_GROW indicates that the FP stack should grow as needed, 
This is the default.

FLOAT_STACK_WRAP indicates that the FP stack should "wrap around" and 
overwrite existing numbers on the bottom of the stack as needed.

FLOAT_STACK_ERROR indicates that FP stack should initiate an error 
whenever it reaches its maximum size.

**Library:** math.def

----------
#### FloatingKeyboardInfo
    FloatingKeyboardInfo    struct
        FKI_defaultPosition     word
            ; If set, the keyboard will be moved to the default position before
            ; being brought onscreen
        FKI_sysModal            word
            ; If set, the currently focused window is system modal, so the
            ; window needs to be moved to a higher layer
    FloatingKeyboardInfo    ends

**Library:** gAppC.def

----------
#### FontAttrs
    FontAttrs       record
        FA_USEFUL           FontUseful:1        ;TRUE: "useful" font
        FA_FIXED_WIDTH      FontPitch:1         ;TRUE: fixed width
        FA_ORIENT           FontOrientation:1   ;TRUE: landscape orientation
        FA_OUTLINE          FontSource:1        ;TRUE: outline defined
        FA_FAMILY           FontFamily:4        ;font family
    FontAttrs       end

**Library:** font.def

----------
#### FontEnumFlags
    FontEnumFlags       record
        FEF_ALPHABETIZE     :1  ;TRUE: alphabetize list
        FEF_USEFUL          :1  ;TRUE: find "useful" fonts only
        FEF_FIXED_WIDTH     :1  ;TRUE: find fixed-width fonts only
        FEF_FAMILY          :1  ;TRUE: match FontFamily
        FEF_STRING          :1  ;TRUE: match string
        FEF_DOWNCASE        :1  ;TRUE: downcase returned strings
        FEF_BITMAPS         :1  ;TRUE: find fonts with bitmaps
        FEF_OUTLINES        :1  ;TRUE: find fonts with outlines.
    FontEnumFlags       end

**Library:** font.def

----------
#### FontEnumStruct
    FontEnumStruct      struct
        FES_ID      FontID
        FES_name    char FONT_NAME_LEN dup (?)  ; null terminated string
    FontEnumStruct      ends

This structure is returned by **GrEnumFonts**.

**Library:** font.def

----------
#### FontFamily
    FontFamily          etype   byte
        FF_SERIF            enum FontFamily
        FF_SANS_SERIF       enum FontFamily
        FF_SCRIPT           enum FontFamily
        FF_ORNAMENT         enum FontFamily
        FF_SYMBOL           enum FontFamily
        FF_MONO             enum FontFamily
        FF_SPECIAL          enum FontFamily
        FF_NON_PORTABLE     enum FontFamily

**Library:** fontID.def

----------
#### FontGroup
    FontGroup       etype   word, 0, FID_FAMILY_DIVISIONS
        FG_SERIF        enum FontGroup, FF_SERIF        *FID_FAMILY_DIVISIONS
        FG_SANS_SERIF   enum FontGroup, FF_SANS_SERIF   *FID_FAMILY_DIVISIONS
        FG_SCRIPT       enum FontGroup, FF_SCRIPT       *FID_FAMILY_DIVISIONS
        FG_ORNAMENT     enum FontGroup, FF_ORNAMENT     *FID_FAMILY_DIVISIONS
        FG_SYMBOL       enum FontGroup, FF_SYMBOL       *FID_FAMILY_DIVISIONS
        FG_MONO         enum FontGroup, FF_MONO         *FID_FAMILY_DIVISIONS
        FG_SPECIAL      enum FontGroup, FF_SPECIAL      *FID_FAMILY_DIVISIONS
        FG_NON_PORTABLE enum FontGroup, FF_NON_PORTABLE *FID_FAMILY_DIVISIONS

**Library:** fontID.def

----------
#### FontID
    FontID  etype   word
        FID_INVALID                         enum FontID, 0x0000 ; invalid font ID
        FID_PRINTER_PROP_SANS               enum FontID,  0xf200
        FID_PRINTER_PROP_SERIF              enum FontID,  0xf000
        FID_BITSTREAM_LETTER_GOTHIC         enum FontID,  0x3a03
        FID_PS_LETTER_GOTHIC                enum FontID,  0x2a03
        FID_DTC_LETTER_GOTHIC               enum FontID,  0x1a03
        FID_BITSTREAM_PRESTIGE_ELITE        enum FontID,  0x3a02
        FID_PS_PRESTIGE_ELITE               enum FontID,  0x2a02
        FID_DTC_PRESTIGE_ELITE              enum FontID,  0x1a02
        FID_BITSTREAM_AMERICAN_TYPEWRITER   enum FontID,  0x3a01
        FID_PS_AMERICAN_TYPEWRITER          enum FontID,  0x2a01
        FID_DTC_AMERICAN_TYPEWRITER         enum FontID,  0x1a01
        FID_BITSTREAM_URW_MONO              enum FontID,  0x3a00
        FID_PS_COURIER                      enum FontID,  0x2a00
        FID_DTC_URW_MONO                    enum FontID,  0x1a00
        FID_BITSTREAM_FUN_DINGBATS          enum FontID,  0x380d
        FID_PS_FUN_DINGBATS                 enum FontID,  0x280d
        FID_DTC_FUN_DINGBATS                enum FontID,  0x180d
        FID_BITSTREAM_CHEQ                  enum FontID,  0x380c
        FID_PS_CHEQ                         enum FontID,  0x280c
        FID_DTC_CHEQ                        enum FontID,  0x180c
        FID_BITSTREAM_BUNDESBAHN_PI_3       enum FontID,  0x380b
        FID_PS_BUNDESBAHN_PI_3              enum FontID,  0x280b
        FID_DTC_BUNDESBAHN_PI_3             enum FontID,  0x180b
        FID_BITSTREAM_BUNDESBAHN_PI_2       enum FontID,  0x380a
        FID_PS_BUNDESBAHN_PI_2              enum FontID,  0x280a
        FID_DTC_BUNDESBAHN_PI_2             enum FontID,  0x180a
        FID_BITSTREAM_BUNDESBAHN_PI_1       enum FontID,  0x3809
        FID_PS_BUNDESBAHN_PI_1              enum FontID,  0x2809
        FID_DTC_BUNDESBAHN_PI_1             enum FontID,  0x1809
        FID_BITSTREAM_U_GREEK_MATH_PI       enum FontID,  0x3808
        FID_PS_U_GREEK_MATH_PI              enum FontID,  0x2808
        FID_DTC_U_GREEK_MATH_PI             enum FontID,  0x1808
        FID_BITSTREAM_U_NEWS_COMM_PI        enum FontID,  0x3807
        FID_PS_U_NEWS_COMM_PI               enum FontID,  0x2807
        FID_DTC_U_NEWS_COMM_PI              enum FontID,  0x1807
        FID_BITSTREAM_ACE_I                 enum FontID,  0x3806
        FID_PS_ACE_I                        enum FontID,  0x2806
        FID_DTC_ACE_I                       enum FontID,  0x1806
        FID_BITSTREAM_SONATA                enum FontID,  0x3805
        FID_PS_SONATA                       enum FontID,  0x2805
        FID_DTC_SONATA                      enum FontID,  0x1805
        FID_BITSTREAM_CARTA                 enum FontID,  0x3804
        FID_PS_CARTA                        enum FontID,  0x2804
        FID_DTC_CARTA                       enum FontID,  0x1804
        FID_BITSTREAM_MICR                  enum FontID,  0x3803
        FID_PS_MICR                         enum FontID,  0x2803
        FID_DTC_MICR                        enum FontID,  0x1803
        FID_BITSTREAM_ZAPF_DINGBATS         enum FontID,  0x3802
        FID_PS_ZAPF_DINGBATS                enum FontID,  0x2802
        FID_DTC_ZAPF_DINGBATS               enum FontID,  0x1802
        FID_BITSTREAM_DINGBATS              enum FontID,  0x3801
        FID_PS_DINGBATS                     enum FontID,  0x2801
        FID_DTC_DINGBATS                    enum FontID,  0x1801
        FID_BITSTREAM_URW_SYMBOLPS          enum FontID,  0x3800
        FID_PS_SYMBOL                       enum FontID,  0x2800
        FID_DTC_URW_SYMBOLPS                enum FontID,  0x1800
        FID_BITSTREAM_JUNIPER               enum FontID,  0x367f
        FID_PS_JUNIPER                      enum FontID,  0x267f
        FID_DTC_JUNIPER                     enum FontID,  0x167f
        FID_BITSTREAM_COTTONWOOD            enum FontID,  0x367e
        FID_PS_COTTONWOOD                   enum FontID,  0x267e
        FID_DTC_COTTONWOOD                  enum FontID,  0x167e
        FID_BITSTREAM_BANCO                 enum FontID,  0x367d
        FID_PS_BANCO                        enum FontID,  0x267d
        FID_DTC_BANCO                       enum FontID,  0x167d
        FID_BITSTREAM_ARCADIA               enum FontID,  0x367c
        FID_PS_ARCADIA                      enum FontID,  0x267c
        FID_DTC_ARCADIA                     enum FontID,  0x167c
        FID_BITSTREAM_ZIPPER                enum FontID,  0x367b
        FID_PS_ZIPPER                       enum FontID,  0x267b
        FID_DTC_ZIPPER                      enum FontID,  0x167b
        FID_BITSTREAM_WEIFZ_RUNDGOTIFCH     enum FontID,  0x367a
        FID_PS_WEIFZ_RUNDGOTIFCH            enum FontID,  0x267a
        FID_DTC_WEIFZ_RUNDGOTIFCH           enum FontID,  0x167a
        FID_BITSTREAM_WASHINGTON            enum FontID,  0x3679
        FID_PS_WASHINGTON                   enum FontID,  0x2679
        FID_DTC_WASHINGTON                  enum FontID,  0x1679
        FID_BITSTREAM_VICTORIAN             enum FontID,  0x3678
        FID_PS_VICTORIAN                    enum FontID,  0x2678
        FID_DTC_VICTORIAN                   enum FontID,  0x1678
        FID_BITSTREAM_VEGAS                 enum FontID,  0x3677
        FID_PS_VEGAS                        enum FontID,  0x2677
        FID_DTC_VEGAS                       enum FontID,  0x1677
        FID_BITSTREAM_VARIO                 enum FontID,  0x3676
        FID_PS_VARIO                        enum FontID,  0x2676
        FID_DTC_VARIO                       enum FontID,  0x1676
        FID_BITSTREAM_VAG_RUNDSCHRIFT       enum FontID,  0x3675
        FID_PS_VAG_RUNDSCHRIFT              enum FontID,  0x2675
        FID_DTC_VAG_RUNDSCHRIFT             enum FontID,  0x1675
        FID_BITSTREAM_TRAJANUS              enum FontID,  0x3674
        FID_PS_TRAJANUS                     enum FontID,  0x2674
        FID_DTC_TRAJANUS                    enum FontID,  0x1674
        FID_BITSTREAM_TITUS                 enum FontID,  0x3673
        FID_PS_TITUS                        enum FontID,  0x2673
        FID_DTC_TITUS                       enum FontID,  0x1673
        FID_BITSTREAM_TIME_SCRIPT           enum FontID,  0x3672
        FID_PS_TIME_SCRIPT                  enum FontID,  0x2672
        FID_DTC_TIME_SCRIPT                 enum FontID,  0x1672
        FID_BITSTREAM_THUNDERBIRD           enum FontID,  0x3671
        FID_PS_THUNDERBIRD                  enum FontID,  0x2671
        FID_DTC_THUNDERBIRD                 enum FontID,  0x1671
        FID_BITSTREAM_THOROWGOOD            enum FontID,  0x3670
        FID_PS_THOROWGOOD                   enum FontID,  0x2670
        FID_DTC_THOROWGOOD                  enum FontID,  0x1670
        FID_BITSTREAM_TARRAGON              enum FontID,  0x366f
        FID_PS_TARRAGON                     enum FontID,  0x266f
        FID_DTC_TARRAGON                    enum FontID,  0x166f
        FID_BITSTREAM_TANGO                 enum FontID,  0x366e
        FID_PS_TANGO                        enum FontID,  0x266e
        FID_DTC_TANGO                       enum FontID,  0x166e
        FID_BITSTREAM_SYNCHRO               enum FontID,  0x366d
        FID_PS_SYNCHRO                      enum FontID,  0x266d
        FID_DTC_SYNCHRO                     enum FontID,  0x166d
        FID_BITSTREAM_SUPERSTAR             enum FontID,  0x366c
        FID_PS_SUPERSTAR                    enum FontID,  0x266c
        FID_DTC_SUPERSTAR                   enum FontID,  0x166c
        FID_BITSTREAM_STOP                  enum FontID,  0x366b
        FID_PS_STOP                         enum FontID,  0x266b
        FID_DTC_STOP                        enum FontID,  0x166b
        FID_BITSTREAM_STILLA_CAPS           enum FontID,  0x366a
        FID_PS_STILLA_CAPS                  enum FontID,  0x266a
        FID_DTC_STILLA_CAPS                 enum FontID,  0x166a
        FID_BITSTREAM_STILLA                enum FontID,  0x3669
        FID_PS_STILLA                       enum FontID,  0x2669
        FID_DTC_STILLA                      enum FontID,  0x1669
        FID_BITSTREAM_STENTOR               enum FontID,  0x3668
        FID_PS_STENTOR                      enum FontID,  0x2668
        FID_DTC_STENTOR                     enum FontID,  0x1668
        FID_BITSTREAM_SQUIRE                enum FontID,  0x3667
        FID_PS_SQUIRE                       enum FontID,  0x2667
        FID_DTC_SQUIRE                      enum FontID,  0x1667
        FID_BITSTREAM_SPRINGFIELD           enum FontID,  0x3666
        FID_PS_SPRINGFIELD                  enum FontID,  0x2666
        FID_DTC_SPRINGFIELD                 enum FontID,  0x1666
        FID_BITSTREAM_SLIPSTREAM            enum FontID,  0x3665
        FID_PS_SLIPSTREAM                   enum FontID,  0x2665
        FID_DTC_SLIPSTREAM                  enum FontID,  0x1665
        FID_BITSTREAM_SINALOA               enum FontID,  0x3664
        FID_PS_SINALOA                      enum FontID,  0x2664
        FID_DTC_SINALOA                     enum FontID,  0x1664
        FID_BITSTREAM_SHELLEY               enum FontID,  0x3663
        FID_PS_SHELLEY                      enum FontID,  0x2663
        FID_DTC_SHELLEY                     enum FontID,  0x1663
        FID_BITSTREAM_SERPENTINE            enum FontID,  0x3662
        FID_PS_SERPENTINE                   enum FontID,  0x2662
        FID_DTC_SERPENTINE                  enum FontID,  0x1662
        FID_BITSTREAM_RUBBER_STAMP          enum FontID,  0x3661
        FID_PS_RUBBER_STAMP                 enum FontID,  0x2661
        FID_DTC_RUBBER_STAMP                enum FontID,  0x1661
        FID_BITSTREAM_ROMIC                 enum FontID,  0x3660
        FID_PS_ROMIC                        enum FontID,  0x2660
        FID_DTC_ROMIC                       enum FontID,  0x1660
        FID_BITSTREAM_RIALTO                enum FontID,  0x365f
        FID_PS_RIALTO                       enum FontID,  0x265f
        FID_DTC_RIALTO                      enum FontID,  0x165f
        FID_BITSTREAM_REVUE                 enum FontID,  0x365e
        FID_PS_REVUE                        enum FontID,  0x265e
        FID_DTC_REVUE                       enum FontID,  0x165e
        FID_BITSTREAM_QUENTIN               enum FontID,  0x365d
        FID_PS_QUENTIN                      enum FontID,  0x265d
        FID_DTC_QUENTIN                     enum FontID,  0x165d
        FID_BITSTREAM_PRO_ARTE              enum FontID,  0x365c
        FID_PS_PRO_ARTE                     enum FontID,  0x265c
        FID_DTC_PRO_ARTE                    enum FontID,  0x165c
        FID_BITSTREAM_PRINCETOWN            enum FontID,  0x365b
        FID_PS_PRINCETOWN                   enum FontID,  0x265b
        FID_DTC_PRINCETOWN                  enum FontID,  0x165b
        FID_BITSTREAM_PRESIDENT             enum FontID,  0x365a
        FID_PS_PRESIDENT                    enum FontID,  0x265a
        FID_DTC_PRESIDENT                   enum FontID,  0x165a
        FID_BITSTREAM_PREMIER               enum FontID,  0x3659
        FID_PS_PREMIER                      enum FontID,  0x2659
        FID_DTC_PREMIER                     enum FontID,  0x1659
        FID_BITSTREAM_POST_ANTIQUA          enum FontID,  0x3658
        FID_PS_POST_ANTIQUA                 enum FontID,  0x2658
        FID_DTC_POST_ANTIQUA                enum FontID,  0x1658
        FID_BITSTREAM_PLAZA                 enum FontID,  0x3657
        FID_PS_PLAZA                        enum FontID,  0x2657
        FID_DTC_PLAZA                       enum FontID,  0x1657
        FID_BITSTREAM_PLAYBILL              enum FontID,  0x3656
        FID_PS_PLAYBILL                     enum FontID,  0x2656
        FID_DTC_PLAYBILL                    enum FontID,  0x1656
        FID_BITSTREAM_PICCADILLY            enum FontID,  0x3655
        FID_PS_PICCADILLY                   enum FontID,  0x2655
        FID_DTC_PICCADILLY                  enum FontID,  0x1655
        FID_BITSTREAM_PEIGNOT               enum FontID,  0x3654
        FID_PS_PEIGNOT                      enum FontID,  0x2654
        FID_DTC_PEIGNOT                     enum FontID,  0x1654
        FID_BITSTREAM_PAPYRUS               enum FontID,  0x3653
        FID_PS_PAPYRUS                      enum FontID,  0x2653
        FID_DTC_PAPYRUS                     enum FontID,  0x1653
        FID_BITSTREAM_PADDINGTION           enum FontID,  0x3652
        FID_PS_PADDINGTION                  enum FontID,  0x2652
        FID_DTC_PADDINGTION                 enum FontID,  0x1652
        FID_BITSTREAM_OKAY                  enum FontID,  0x3651
        FID_PS_OKAY                         enum FontID,  0x2651
        FID_DTC_OKAY                        enum FontID,  0x1651
        FID_BITSTREAM_ODIN                  enum FontID,  0x3650
        FID_PS_ODIN                         enum FontID,  0x2650
        FID_DTC_ODIN                        enum FontID,  0x1650
        FID_BITSTREAM_OCTOPUSS              enum FontID,  0x364f
        FID_PS_OCTOPUSS                     enum FontID,  0x264f
        FID_DTC_OCTOPUSS                    enum FontID,  0x164f
        FID_BITSTREAM_MOTTER_FEMINA         enum FontID,  0x364e
        FID_PS_MOTTER_FEMINA                enum FontID,  0x264e
        FID_DTC_MOTTER_FEMINA               enum FontID,  0x164e
        FID_BITSTREAM_MICROGRAMMA           enum FontID,  0x364d
        FID_PS_MICROGRAMMA                  enum FontID,  0x264d
        FID_DTC_MICROGRAMMA                 enum FontID,  0x164d
        FID_BITSTREAM_MACHINE               enum FontID,  0x364c
        FID_PS_MACHINE                      enum FontID,  0x264c
        FID_DTC_MACHINE                     enum FontID,  0x164c
        FID_BITSTREAM_LINOTEXT              enum FontID,  0x364b
        FID_PS_LINOTEXT                     enum FontID,  0x264b
        FID_DTC_LINOTEXT                    enum FontID,  0x164b
        FID_BITSTREAM_LIBERTY               enum FontID,  0x364a
        FID_PS_LIBERTY                      enum FontID,  0x264a
        FID_DTC_LIBERTY                     enum FontID,  0x164a
        FID_BITSTREAM_LAZYBONES             enum FontID,  0x3649
        FID_PS_LAZYBONES                    enum FontID,  0x2649
        FID_DTC_LAZYBONES                   enum FontID,  0x1649
        FID_BITSTREAM_LATIN_WIDE            enum FontID,  0x3648
        FID_PS_LATIN_WIDE                   enum FontID,  0x2648
        FID_DTC_LATIN_WIDE                  enum FontID,  0x1648
        FID_BITSTREAM_KNIGHTSBRIDGE         enum FontID,  0x3647
        FID_PS_KNIGHTSBRIDGE                enum FontID,  0x2647
        FID_DTC_KNIGHTSBRIDGE               enum FontID,  0x1647
        FID_BITSTREAM_KAPITELLIA            enum FontID,  0x3646
        FID_PS_KAPITELLIA                   enum FontID,  0x2646
        FID_DTC_KAPITELLIA                  enum FontID,  0x1646
        FID_BITSTREAM_KALLIGRAPHIA          enum FontID,  0x3645
        FID_PS_KALLIGRAPHIA                 enum FontID,  0x2645
        FID_DTC_KALLIGRAPHIA                enum FontID,  0x1645
        FID_BITSTREAM_ICE_AGE               enum FontID,  0x3644
        FID_PS_ICE_AGE                      enum FontID,  0x2644
        FID_DTC_ICE_AGE                     enum FontID,  0x1644
        FID_BITSTREAM_ICONE                 enum FontID,  0x3643
        FID_PS_ICONE                        enum FontID,  0x2643
        FID_DTC_ICONE                       enum FontID,  0x1643
        FID_BITSTREAM_HORNDON               enum FontID,  0x3642
        FID_PS_HORNDON                      enum FontID,  0x2642
        FID_DTC_HORNDON                     enum FontID,  0x1642
        FID_BITSTREAM_HORATIO               enum FontID,  0x3641
        FID_PS_HORATIO                      enum FontID,  0x2641
        FID_DTC_HORATIO                     enum FontID,  0x1641
        FID_BITSTREAM_HIGHLIGHT             enum FontID,  0x3640
        FID_PS_HIGHLIGHT                    enum FontID,  0x2640
        FID_DTC_HIGHLIGHT                   enum FontID,  0x1640
        FID_BITSTREAM_HADFIELD              enum FontID,  0x363f
        FID_PS_HADFIELD                     enum FontID,  0x263f
        FID_DTC_HADFIELD                    enum FontID,  0x163f
        FID_BITSTREAM_GLASER_STENCIL        enum FontID,  0x363e
        FID_PS_GLASER_STENCIL               enum FontID,  0x263e
        FID_DTC_GLASER_STENCIL              enum FontID,  0x163e
        FID_BITSTREAM_GILL_KAYO             enum FontID,  0x363d
        FID_PS_GILL_KAYO                    enum FontID,  0x263d
        FID_DTC_GILL_KAYO                   enum FontID,  0x163d
        FID_BITSTREAM_GALADRIEL             enum FontID,  0x363c
        FID_PS_GALADRIEL                    enum FontID,  0x263c
        FID_DTC_GALADRIEL                   enum FontID,  0x163c
        FID_BITSTREAM_FUTURA_DISPLAY        enum FontID,  0x363b
        FID_PS_FUTURA_DISPLAY               enum FontID,  0x263b
        FID_DTC_FUTURA_DISPLAY              enum FontID,  0x163b
        FID_BITSTREAM_FUTURA_C_BLACK        enum FontID,  0x363a
        FID_PS_FUTURA_C_BLACK               enum FontID,  0x263a
        FID_DTC_FUTURA_C_BLACK              enum FontID,  0x163a
        FID_BITSTREAM_FRANKFURTER           enum FontID,  0x3639
        FID_PS_FRANKFURTER                  enum FontID,  0x2639
        FID_DTC_FRANKFURTER                 enum FontID,  0x1639
        FID_BITSTREAM_FLORA                 enum FontID,  0x3638
        FID_PS_FLORA                        enum FontID,  0x2638
        FID_DTC_FLORA                       enum FontID,  0x1638
        FID_BITSTREAM_FLANGE                enum FontID,  0x3637
        FID_PS_FLANGE                       enum FontID,  0x2637
        FID_DTC_FLANGE                      enum FontID,  0x1637
        FID_BITSTREAM_FLASH                 enum FontID,  0x3636
        FID_PS_FLASH                        enum FontID,  0x2636
        FID_DTC_FLASH                       enum FontID,  0x1636
        FID_BITSTREAM_FLAMENCO              enum FontID,  0x3635
        FID_PS_FLAMENCO                     enum FontID,  0x2635
        FID_DTC_FLAMENCO                    enum FontID,  0x1635
        FID_BITSTREAM_FETTE_GOTILCH         enum FontID,  0x3634
        FID_PS_FETTE_GOTILCH                enum FontID,  0x2634
        FID_DTC_FETTE_GOTILCH               enum FontID,  0x1634
        FID_BITSTREAM_FETTE_FRAKTUR         enum FontID,  0x3633
        FID_PS_FETTE_FRAKTUR                enum FontID,  0x2633
        FID_DTC_FETTE_FRAKTUR               enum FontID,  0x1633
        FID_BITSTREAM_ENVIRO                enum FontID,  0x3632
        FID_PS_ENVIRO                       enum FontID,  0x2632
        FID_DTC_ENVIRO                      enum FontID,  0x1632
        FID_BITSTREAM_EINHORN               enum FontID,  0x3631
        FID_PS_EINHORN                      enum FontID,  0x2631
        FID_DTC_EINHORN                     enum FontID,  0x1631
        FID_BITSTREAM_ECKMANN               enum FontID,  0x3630
        FID_PS_ECKMANN                      enum FontID,  0x2630
        FID_DTC_ECKMANN                     enum FontID,  0x1630
        FID_BITSTREAM_DYNAMO                enum FontID,  0x362f
        FID_PS_DYNAMO                       enum FontID,  0x262f
        FID_DTC_DYNAMO                      enum FontID,  0x162f
        FID_BITSTREAM_DOM_CASUAL            enum FontID,  0x362e
        FID_PS_DOM_CASUAL                   enum FontID,  0x262e
        FID_DTC_DOM_CASUAL                  enum FontID,  0x162e
        FID_BITSTREAM_DAVIDA                enum FontID,  0x362d
        FID_PS_DAVIDA                       enum FontID,  0x262d
        FID_DTC_DAVIDA                      enum FontID,  0x162d
        FID_BITSTREAM_CROISSANT             enum FontID,  0x362c
        FID_PS_CROISSANT                    enum FontID,  0x262c
        FID_DTC_CROISSANT                   enum FontID,  0x162c
        FID_BITSTREAM_CRILLEE               enum FontID,  0x362b
        FID_PS_CRILLEE                      enum FontID,  0x262b
        FID_DTC_CRILLEE                     enum FontID,  0x162b
        FID_BITSTREAM_COUNTDOWN             enum FontID,  0x362a
        FID_PS_COUNTDOWN                    enum FontID,  0x262a
        FID_DTC_COUNTDOWN                   enum FontID,  0x162a
        FID_BITSTREAM_CORTEZ                enum FontID,  0x3629
        FID_PS_CORTEZ                       enum FontID,  0x2629
        FID_DTC_CORTEZ                      enum FontID,  0x1629
        FID_BITSTREAM_CONFERENCE            enum FontID,  0x3628
        FID_PS_CONFERENCE                   enum FontID,  0x2628
        FID_DTC_CONFERENCE                  enum FontID,  0x1628
        FID_BITSTREAM_COMPANY               enum FontID,  0x3627
        FID_PS_COMPANY                      enum FontID,  0x2627
        FID_DTC_COMPANY                     enum FontID,  0x1627
        FID_BITSTREAM_COLUMNA_SOLID         enum FontID,  0x3626
        FID_PS_COLUMNA_SOLID                enum FontID,  0x2626
        FID_DTC_COLUMNA_SOLID               enum FontID,  0x1626
        FID_BITSTREAM_CITY                  enum FontID,  0x3625
        FID_PS_CITY                         enum FontID,  0x2625
        FID_DTC_CITY                        enum FontID,  0x1625
        FID_BITSTREAM_CIRKULUS              enum FontID,  0x3624
        FID_PS_CIRKULUS                     enum FontID,  0x2624
        FID_DTC_CIRKULUS                    enum FontID,  0x1624
        FID_BITSTREAM_CHURCHWARD_BRUSH      enum FontID,  0x3623
        FID_PS_CHURCHWARD_BRUSH             enum FontID,  0x2623
        FID_DTC_CHURCHWARD_BRUSH            enum FontID,  0x1623
        FID_BITSTREAM_CHROMIUM_ONE          enum FontID,  0x3622
        FID_PS_CHROMIUM_ONE                 enum FontID,  0x2622
        FID_DTC_CHROMIUM_ONE                enum FontID,  0x1622
        FID_BITSTREAM_CHOC                  enum FontID,  0x3621
        FID_PS_CHOC                         enum FontID,  0x2621
        FID_DTC_CHOC                        enum FontID,  0x1621
        FID_BITSTREAM_CHISEL                enum FontID,  0x3620
        FID_PS_CHISEL                       enum FontID,  0x2620
        FID_DTC_CHISEL                      enum FontID,  0x1620
        FID_BITSTREAM_CHESTERFIELD          enum FontID,  0x361f
        FID_PS_CHESTERFIELD                 enum FontID,  0x261f
        FID_DTC_CHESTERFIELD                enum FontID,  0x161f
        FID_BITSTREAM_CAROUSEL              enum FontID,  0x361e
        FID_PS_CAROUSEL                     enum FontID,  0x261e
        FID_DTC_CAROUSEL                    enum FontID,  0x161e
        FID_BITSTREAM_CAMELLIA              enum FontID,  0x361d
        FID_PS_CAMELLIA                     enum FontID,  0x261d
        FID_DTC_CAMELLIA                    enum FontID,  0x161d
        FID_BITSTREAM_CABARET               enum FontID,  0x361c
        FID_PS_CABARET                      enum FontID,  0x261c
        FID_DTC_CABARET                     enum FontID,  0x161c
        FID_BITSTREAM_BUXOM                 enum FontID,  0x361b
        FID_PS_BUXOM                        enum FontID,  0x261b
        FID_DTC_BUXOM                       enum FontID,  0x161b
        FID_BITSTREAM_BUSTER                enum FontID,  0x361a
        FID_PS_BUSTER                       enum FontID,  0x261a
        FID_DTC_BUSTER                      enum FontID,  0x161a
        FID_BITSTREAM_BOTTLENECK            enum FontID,  0x3619
        FID_PS_BOTTLENECK                   enum FontID,  0x2619
        FID_DTC_BOTTLENECK                  enum FontID,  0x1619
        FID_BITSTREAM_BLOCK                 enum FontID,  0x3618
        FID_PS_BLOCK                        enum FontID,  0x2618
        FID_DTC_BLOCK                       enum FontID,  0x1618
        FID_BITSTREAM_BINNER                enum FontID,  0x3617
        FID_PS_BINNER                       enum FontID,  0x2617
        FID_DTC_BINNER                      enum FontID,  0x1617
        FID_BITSTREAM_BERNHARD_ANTIQUE      enum FontID,  0x3616
        FID_PS_BERNHARD_ANTIQUE             enum FontID,  0x2616
        FID_DTC_BERNHARD_ANTIQUE            enum FontID,  0x1616
        FID_BITSTREAM_BELSHAW               enum FontID,  0x3615
        FID_PS_BELSHAW                      enum FontID,  0x2615
        FID_DTC_BELSHAW                     enum FontID,  0x1615
        FID_BITSTREAM_BARCELONA             enum FontID,  0x3614
        FID_PS_BARCELONA                    enum FontID,  0x2614
        FID_DTC_BARCELONA                   enum FontID,  0x1614
        FID_BITSTREAM_BAUHAUS               enum FontID,  0x3613
        FID_PS_BAUHAUS                      enum FontID,  0x2613
        FID_DTC_BAUHAUS                     enum FontID,  0x1613
        FID_BITSTREAM_AUGUSTEA_OPEN         enum FontID,  0x3612
        FID_PS_AUGUSTEA_OPEN                enum FontID,  0x2612
        FID_DTC_AUGUSTEA_OPEN               enum FontID,  0x1612
        FID_BITSTREAM_AMERICAN_UNCIAL       enum FontID,  0x3611
        FID_PS_AMERICAN_UNCIAL              enum FontID,  0x2611
        FID_DTC_AMERICAN_UNCIAL             enum FontID,  0x1611
        FID_BITSTREAM_ULTE_SCHWABACHER      enum FontID,  0x3610
        FID_PS_ULTE_SCHWABACHER             enum FontID,  0x2610
        FID_DTC_ULTE_SCHWABACHER            enum FontID,  0x1610
        FID_BITSTREAM_ARNOLD_BOCKLIN        enum FontID,  0x360f
        FID_PS_ARNOLD_BOCKLIN               enum FontID,  0x260f
        FID_DTC_ARNOLD_BOCKLIN              enum FontID,  0x160f
        FID_BITSTREAM_ALGERIAN              enum FontID,  0x360e
        FID_PS_ALGERIAN                     enum FontID,  0x260e
        FID_DTC_ALGERIAN                    enum FontID,  0x160e
        FID_BITSTREAM_PUMP                  enum FontID,  0x360d
        FID_PS_PUMP                         enum FontID,  0x260d
        FID_DTC_PUMP                        enum FontID,  0x160d
        FID_BITSTREAM_MARIAGE               enum FontID,  0x360c
        FID_PS_MARIAGE                      enum FontID,  0x260c
        FID_DTC_MARIAGE                     enum FontID,  0x160c
        FID_BITSTREAM_OLD_TOWN              enum FontID,  0x360b
        FID_PS_OLD_TOWN                     enum FontID,  0x260b
        FID_DTC_OLD_TOWN                    enum FontID,  0x160b
        FID_BITSTREAM_HOBO                  enum FontID,  0x360a
        FID_PS_HOBO                         enum FontID,  0x260a
        FID_DTC_HOBO                        enum FontID,  0x160a
        FID_BITSTREAM_GOUDY_HEAVYFACE       enum FontID,  0x3609
        FID_PS_GOUDY_HEAVYFACE              enum FontID,  0x2609
        FID_DTC_GOUDY_HEAVYFACE             enum FontID,  0x1609
        FID_BITSTREAM_DATA_70               enum FontID,  0x3608
        FID_PS_DATA_70                      enum FontID,  0x2608
        FID_DTC_DATA_70                     enum FontID,  0x1608
        FID_BITSTREAM_LCD                   enum FontID,  0x3607
        FID_PS_LCD                          enum FontID,  0x2607
        FID_DTC_LCD                         enum FontID,  0x1607
        FID_BITSTREAM_BALLOON               enum FontID,  0x3606
        FID_PS_BALLOON                      enum FontID,  0x2606
        FID_DTC_BALLOON                     enum FontID,  0x1606
        FID_BITSTREAM_BLIPPO_C_BLACK        enum FontID,  0x3605
        FID_PS_BLIPPO_C_BLACK               enum FontID,  0x2605
        FID_DTC_BLIPPO_C_BLACK              enum FontID,  0x1605
        FID_BITSTREAM_COOPER_C_BLACK        enum FontID,  0x3604
        FID_PS_COOPER_C_BLACK               enum FontID,  0x2604
        FID_DTC_COOPER_C_BLACK              enum FontID,  0x1604
        FID_BITSTREAM_COPPERPLATE           enum FontID,  0x3603
        FID_PS_COPPERPLATE                  enum FontID,  0x2603
        FID_DTC_COPPERPLATE                 enum FontID,  0x1603
        FID_BITSTREAM_STENCIL               enum FontID,  0x3602
        FID_PS_STENCIL                      enum FontID,  0x2602
        FID_DTC_STENCIL                     enum FontID,  0x1602
        FID_BITSTREAM_OLD_ENGLISH           enum FontID,  0x3601
        FID_PS_OLD_ENGLISH                  enum FontID,  0x2601
        FID_DTC_OLD_ENGLISH                 enum FontID,  0x1601
        FID_BITSTREAM_BROADWAY              enum FontID,  0x3600
        FID_PS_BROADWAY                     enum FontID,  0x2600
        FID_DTC_BROADWAY                    enum FontID,  0x1600
        FID_BITSTREAM_NUPITAL_SCRIPT        enum FontID,  0x3430
        FID_PS_NUPITAL_SCRIPT               enum FontID,  0x2430
        FID_DTC_NUPITAL_SCRIPT              enum FontID,  0x1430
        FID_BITSTREAM_MEDICI_SCRIPT         enum FontID,  0x342f
        FID_PS_MEDICI_SCRIPT                enum FontID,  0x242f
        FID_DTC_MEDICI_SCRIPT               enum FontID,  0x142f
        FID_BITSTREAM_CHARME                enum FontID,  0x342e
        FID_PS_CHARME                       enum FontID,  0x242e
        FID_DTC_CHARME                      enum FontID,  0x142e
        FID_BITSTREAM_CASCADE_SCRIPT        enum FontID,  0x342d
        FID_PS_CASCADE_SCRIPT               enum FontID,  0x242d
        FID_DTC_CASCADE_SCRIPT              enum FontID,  0x142d
        FID_BITSTREAM_LITHOS                enum FontID,  0x342c
        FID_PS_LITHOS                       enum FontID,  0x242c
        FID_DTC_LITHOS                      enum FontID,  0x142c
        FID_BITSTREAM_TEKTON                enum FontID,  0x342b
        FID_PS_TEKTON                       enum FontID,  0x242b
        FID_DTC_TEKTON                      enum FontID,  0x142b
        FID_BITSTREAM_VLADIMIR_SCRIPT       enum FontID,  0x342a
        FID_PS_VLADIMIR_SCRIPT              enum FontID,  0x242a
        FID_DTC_VLADIMIR_SCRIPT             enum FontID,  0x142a
        FID_BITSTREAM_VAN_DIJK              enum FontID,  0x3429
        FID_PS_VAN_DIJK                     enum FontID,  0x2429
        FID_DTC_VAN_DIJK                    enum FontID,  0x1429
        FID_BITSTREAM_SLOGAN                enum FontID,  0x3428
        FID_PS_SLOGAN                       enum FontID,  0x2428
        FID_DTC_SLOGAN                      enum FontID,  0x1428
        FID_BITSTREAM_SHAMROCK              enum FontID,  0x3427
        FID_PS_SHAMROCK                     enum FontID,  0x2427
        FID_DTC_SHAMROCK                    enum FontID,  0x1427
        FID_BITSTREAM_ROMAN_SCRIPT          enum FontID,  0x3426
        FID_PS_ROMAN_SCRIPT                 enum FontID,  0x2426
        FID_DTC_ROMAN_SCRIPT                enum FontID,  0x1426
        FID_BITSTREAM_RAGE                  enum FontID,  0x3425
        FID_PS_RAGE                         enum FontID,  0x2425
        FID_DTC_RAGE                        enum FontID,  0x1425
        FID_BITSTREAM_PRESENT_SCRIPT        enum FontID,  0x3424
        FID_PS_PRESENT_SCRIPT               enum FontID,  0x2424
        FID_DTC_PRESENT_SCRIPT              enum FontID,  0x1424
        FID_BITSTREAM_PHYLLIS_INITIALS      enum FontID,  0x3423
        FID_PS_PHYLLIS_INITIALS             enum FontID,  0x2423
        FID_DTC_PHYLLIS_INITIALS            enum FontID,  0x1423
        FID_BITSTREAM_PHYLLIS               enum FontID,  0x3422
        FID_PS_PHYLLIS                      enum FontID,  0x2422
        FID_DTC_PHYLLIS                     enum FontID,  0x1422
        FID_BITSTREAM_PEPITA                enum FontID,  0x3421
        FID_PS_PEPITA                       enum FontID,  0x2421
        FID_DTC_PEPITA                      enum FontID,  0x1421
        FID_BITSTREAM_PENDRY_SCRIPT         enum FontID,  0x3420
        FID_PS_PENDRY_SCRIPT                enum FontID,  0x2420
        FID_DTC_PENDRY_SCRIPT               enum FontID,  0x1420
        FID_BITSTREAM_PALETTE               enum FontID,  0x341f
        FID_PS_PALETTE                      enum FontID,  0x241f
        FID_DTC_PALETTE                     enum FontID,  0x141f
        FID_BITSTREAM_PALACE_SCRIPT         enum FontID,  0x341e
        FID_PS_PALACE_SCRIPT                enum FontID,  0x241e
        FID_DTC_PALACE_SCRIPT               enum FontID,  0x141e
        FID_BITSTREAM_NEVISON_CASUAL        enum FontID,  0x341d
        FID_PS_NEVISON_CASUAL               enum FontID,  0x241d
        FID_DTC_NEVISON_CASUAL              enum FontID,  0x141d
        FID_BITSTREAM_HILL                  enum FontID,  0x341c
        FID_PS_HILL                         enum FontID,  0x241c
        FID_DTC_HILL                        enum FontID,  0x141c
        FID_BITSTREAM_LINOSCRIPT            enum FontID,  0x341b
        FID_PS_LINOSCRIPT                   enum FontID,  0x241b
        FID_DTC_LINOSCRIPT                  enum FontID,  0x141b
        FID_BITSTREAM_LINDSAY               enum FontID,  0x341a
        FID_PS_LINDSAY                      enum FontID,  0x241a
        FID_DTC_LINDSAY                     enum FontID,  0x141a
        FID_BITSTREAM_LE_GRIFFE             enum FontID,  0x3419
        FID_PS_LE_GRIFFE                    enum FontID,  0x2419
        FID_DTC_LE_GRIFFE                   enum FontID,  0x1419
        FID_BITSTREAM_KUNSTLERSCHREIBSCHRIFT enum FontID,  0x3418
        FID_PS_KUNSTLERSCHREIBSCHRIFT       enum FontID,  0x2418
        FID_DTC_KUNSTLERSCHREIBSCHRIFT      enum FontID,  0x1418
        FID_BITSTREAM_JULIA_SCRIPT          enum FontID,  0x3417
        FID_PS_JULIA_SCRIPT                 enum FontID,  0x2417
        FID_DTC_JULIA_SCRIPT                enum FontID,  0x1417
        FID_BITSTREAM_ISBELL                enum FontID,  0x3416
        FID_PS_ISBELL                       enum FontID,  0x2416
        FID_DTC_ISBELL                      enum FontID,  0x1416
        FID_BITSTREAM_ISADORA               enum FontID,  0x3415
        FID_PS_ISADORA                      enum FontID,  0x2415
        FID_DTC_ISADORA                     enum FontID,  0x1415
        FID_BITSTREAM_HOGARTH_SCRIPT        enum FontID,  0x3414
        FID_PS_HOGARTH_SCRIPT               enum FontID,  0x2414
        FID_DTC_HOGARTH_SCRIPT              enum FontID,  0x1414
        FID_BITSTREAM_HARLOW                enum FontID,  0x3413
        FID_PS_HARLOW                       enum FontID,  0x2413
        FID_DTC_HARLOW                      enum FontID,  0x1413
        FID_BITSTREAM_GLASTONBURY           enum FontID,  0x3412
        FID_PS_GLASTONBURY                  enum FontID,  0x2412
        FID_DTC_GLASTONBURY                 enum FontID,  0x1412
        FID_BITSTREAM_GILLIES_GOTHIC        enum FontID,  0x3411
        FID_PS_GILLIES_GOTHIC               enum FontID,  0x2411
        FID_DTC_GILLIES_GOTHIC              enum FontID,  0x1411
        FID_BITSTREAM_FREESTYLE_SCRIPT      enum FontID,  0x3410
        FID_PS_FREESTYLE_SCRIPT             enum FontID,  0x2410
        FID_DTC_FREESTYLE_SCRIPT            enum FontID,  0x1410
        FID_BITSTREAM_ENGLISCHE_SCHREIBSCHRIFT enum FontID,  0x340f
        FID_PS_ENGLISCHE_SCHREIBSCHRIFT     enum FontID,  0x240f
        FID_DTC_ENGLISCHE_SCHREIBSCHRIFT    enum FontID,  0x140f
        FID_BITSTREAM_DEMIAN                enum FontID,  0x340e
        FID_PS_DEMIAN                       enum FontID,  0x240e
        FID_DTC_DEMIAN                      enum FontID,  0x140e
        FID_BITSTREAM_CANDICE               enum FontID,  0x340d
        FID_PS_CANDICE                      enum FontID,  0x240d
        FID_DTC_CANDICE                     enum FontID,  0x140d
        FID_BITSTREAM_BRONX                 enum FontID,  0x340c
        FID_PS_BRONX                        enum FontID,  0x240c
        FID_DTC_BRONX                       enum FontID,  0x140c
        FID_BITSTREAM_BRODY                 enum FontID,  0x340b
        FID_PS_BRODY                        enum FontID,  0x240b
        FID_DTC_BRODY                       enum FontID,  0x140b
        FID_BITSTREAM_BIBLE_SCRIPT          enum FontID,  0x340a
        FID_PS_BIBLE_SCRIPT                 enum FontID,  0x240a
        FID_DTC_BIBLE_SCRIPT                enum FontID,  0x140a
        FID_BITSTREAM_ARISTON               enum FontID,  0x3409
        FID_PS_ARISTON                      enum FontID,  0x2409
        FID_DTC_ARISTON                     enum FontID,  0x1409
        FID_BITSTREAM_ANGLIA                enum FontID,  0x3408
        FID_PS_ANGLIA                       enum FontID,  0x2408
        FID_DTC_ANGLIA                      enum FontID,  0x1408
        FID_BITSTREAM_MISTRAL               enum FontID,  0x3407
        FID_PS_MISTRAL                      enum FontID,  0x2407
        FID_DTC_MISTRAL                     enum FontID,  0x1407
        FID_BITSTREAM_BALMORAL              enum FontID,  0x3406
        FID_PS_BALMORAL                     enum FontID,  0x2406
        FID_DTC_BALMORAL                    enum FontID,  0x1406
        FID_BITSTREAM_COMMERCIAL_SCRIPT     enum FontID,  0x3405
        FID_PS_COMMERCIAL_SCRIPT            enum FontID,  0x2405
        FID_DTC_COMMERCIAL_SCRIPT           enum FontID,  0x1405
        FID_BITSTREAM_KAUFMANN              enum FontID,  0x3404
        FID_PS_KAUFMANN                     enum FontID,  0x2404
        FID_DTC_KAUFMANN                    enum FontID,  0x1404
        FID_BITSTREAM_PARK_AVENUE           enum FontID,  0x3403
        FID_PS_PARK_AVENUE                  enum FontID,  0x2403
        FID_DTC_PARK_AVENUE                 enum FontID,  0x1403
        FID_BITSTREAM_BRUSH_SCRIPT          enum FontID,  0x3402
        FID_PS_BRUSH_SCRIPT                 enum FontID,  0x2402
        FID_DTC_BRUSH_SCRIPT                enum FontID,  0x1402
        FID_BITSTREAM_VIVALDI               enum FontID,  0x3401
        FID_PS_VIVALDI                      enum FontID,  0x2401
        FID_DTC_VIVALDI                     enum FontID,  0x1401
        FID_BITSTREAM_ZAPF_CHANCERY         enum FontID,  0x3400
        FID_PS_ZAPF_CHANCERY                enum FontID,  0x2400
        FID_DTC_ZAPF_CHANCERY               enum FontID,  0x1400
        FID_BITSTREAM_AVANTE_GARDE_CONDENSED enum FontID,  0x323d
        FID_PS_AVANTE_GARDE_CONDENSED       enum FontID,  0x223d
        FID_DTC_AVANTE_GARDE_CONDENSED      enum FontID,  0x123d
        FID_BITSTREAM_INSIGNIA              enum FontID,  0x323c
        FID_PS_INSIGNIA                     enum FontID,  0x223c
        FID_DTC_INSIGNIA                    enum FontID,  0x123c
        FID_BITSTREAM_INDUSTRIA             enum FontID,  0x323b
        FID_PS_INDUSTRIA                    enum FontID,  0x223b
        FID_DTC_INDUSTRIA                   enum FontID,  0x123b
        FID_BITSTREAM_DORIC_BOLD            enum FontID,  0x323a
        FID_PS_DORIC_BOLD                   enum FontID,  0x223a
        FID_DTC_DORIC_BOLD                  enum FontID,  0x123a
        FID_BITSTREAM_AKZINDENZ_GROTESK     enum FontID,  0x3239
        FID_PS_AKZINDENZ_GROTESK            enum FontID,  0x2239
        FID_DTC_AKZINDENZ_GROTESK           enum FontID,  0x1239
        FID_BITSTREAM_GROTESK               enum FontID,  0x3238
        FID_PS_GROTESK                      enum FontID,  0x2238
        FID_DTC_GROTESK                     enum FontID,  0x1238
        FID_BITSTREAM_TEMPO                 enum FontID,  0x3237
        FID_PS_TEMPO                        enum FontID,  0x2237
        FID_DTC_TEMPO                       enum FontID,  0x1237
        FID_BITSTREAM_SYNTAX                enum FontID,  0x3236
        FID_PS_SYNTAX                       enum FontID,  0x2236
        FID_DTC_SYNTAX                      enum FontID,  0x1236
        FID_BITSTREAM_STONE_SANS            enum FontID,  0x3235
        FID_PS_STONE_SANS                   enum FontID,  0x2235
        FID_DTC_STONE_SANS                  enum FontID,  0x1235
        FID_BITSTREAM_SERIF_GOTHIC          enum FontID,  0x3234
        FID_PS_SERIF_GOTHIC                 enum FontID,  0x2234
        FID_DTC_SERIF_GOTHIC                enum FontID,  0x1234
        FID_BITSTREAM_PRIMUS_ANTIQUA        enum FontID,  0x3233
        FID_PS_PRIMUS_ANTIQUA               enum FontID,  0x2233
        FID_DTC_PRIMUS_ANTIQUA              enum FontID,  0x1233
        FID_BITSTREAM_PRIMUS                enum FontID,  0x3232
        FID_PS_PRIMUS                       enum FontID,  0x2232
        FID_DTC_PRIMUS                      enum FontID,  0x1232
        FID_BITSTREAM_PRAXIS                enum FontID,  0x3231
        FID_PS_PRAXIS                       enum FontID,  0x2231
        FID_DTC_PRAXIS                      enum FontID,  0x1231
        FID_BITSTREAM_PANACHE               enum FontID,  0x3230
        FID_PS_PANACHE                      enum FontID,  0x2230
        FID_DTC_PANACHE                     enum FontID,  0x1230
        FID_BITSTREAM_OCR_B                 enum FontID,  0x322f
        FID_PS_OCR_B                        enum FontID,  0x222f
        FID_DTC_OCR_B                       enum FontID,  0x122f
        FID_BITSTREAM_OCR_A                 enum FontID,  0x322e
        FID_PS_OCR_A                        enum FontID,  0x222e
        FID_DTC_OCR_A                       enum FontID,  0x122e
        FID_BITSTREAM_NEWTEXT               enum FontID,  0x322d
        FID_PS_NEWTEXT                      enum FontID,  0x222d
        FID_DTC_NEWTEXT                     enum FontID,  0x122d
        FID_BITSTREAM_NEWS_GOTHIC           enum FontID,  0x322c
        FID_PS_NEWS_GOTHIC                  enum FontID,  0x222c
        FID_DTC_NEWS_GOTHIC                 enum FontID,  0x122c
        FID_BITSTREAM_NEUZEIT_GROTESK       enum FontID,  0x322b
        FID_PS_NEUZEIT_GROTESK              enum FontID,  0x222b
        FID_DTC_NEUZEIT_GROTESK             enum FontID,  0x122b
        FID_BITSTREAM_MIXAGE                enum FontID,  0x322a
        FID_PS_MIXAGE                       enum FontID,  0x222a
        FID_DTC_MIXAGE                      enum FontID,  0x122a
        FID_BITSTREAM_MAXIMA                enum FontID,  0x3229
        FID_PS_MAXIMA                       enum FontID,  0x2229
        FID_DTC_MAXIMA                      enum FontID,  0x1229
        FID_BITSTREAM_LUCIDA_SANS           enum FontID,  0x3228
        FID_PS_LUCIDA_SANS                  enum FontID,  0x2228
        FID_DTC_LUCIDA_SANS                 enum FontID,  0x1228
        FID_BITSTREAM_LITERA                enum FontID,  0x3227
        FID_PS_LITERA                       enum FontID,  0x2227
        FID_DTC_LITERA                      enum FontID,  0x1227
        FID_BITSTREAM_KABEL                 enum FontID,  0x3226
        FID_PS_KABEL                        enum FontID,  0x2226
        FID_DTC_KABEL                       enum FontID,  0x1226
        FID_BITSTREAM_HOLSATIA              enum FontID,  0x3225
        FID_PS_HOLSATIA                     enum FontID,  0x2225
        FID_DTC_HOLSATIA                    enum FontID,  0x1225
        FID_BITSTREAM_HELVETICA_INSERAT     enum FontID,  0x3224
        FID_PS_HELVETICA_INSERAT            enum FontID,  0x2224
        FID_DTC_HELVETICA_INSERAT           enum FontID,  0x1224
        FID_BITSTREAM_NEUE_HELVETICA        enum FontID,  0x3223
        FID_PS_NEUE_HELVETICA               enum FontID,  0x2223
        FID_DTC_NEUE_HELVETICA              enum FontID,  0x1223
        FID_BITSTREAM_HELVETICA             enum FontID,  0x3222
        FID_PS_HELVETICA                    enum FontID,  0x2222
        FID_DTC_HELVETICA                   enum FontID,  0x1222
        FID_BITSTREAM_HAAS_UNICA            enum FontID,  0x3221
        FID_PS_HAAS_UNICA                   enum FontID,  0x2221
        FID_DTC_HAAS_UNICA                  enum FontID,  0x1221
        FID_BITSTREAM_GOUDY_SANS            enum FontID,  0x3220
        FID_PS_GOUDY_SANS                   enum FontID,  0x2220
        FID_DTC_GOUDY_SANS                  enum FontID,  0x1220
        FID_BITSTREAM_GOTHIC                enum FontID,  0x321f
        FID_PS_GOTHIC                       enum FontID,  0x221f
        FID_DTC_GOTHIC                      enum FontID,  0x121f
        FID_BITSTREAM_GILL_SANS             enum FontID,  0x321e
        FID_PS_GILL_SANS                    enum FontID,  0x221e
        FID_DTC_GILL_SANS                   enum FontID,  0x121e
        FID_BITSTREAM_GILL                  enum FontID,  0x321d
        FID_PS_GILL                         enum FontID,  0x221d
        FID_DTC_GILL                        enum FontID,  0x121d
        FID_BITSTREAM_FUTURA                enum FontID,  0x321c
        FID_PS_FUTURA                       enum FontID,  0x221c
        FID_DTC_FUTURA                      enum FontID,  0x121c
        FID_BITSTREAM_FOLIO                 enum FontID,  0x321b
        FID_PS_FOLIO                        enum FontID,  0x221b
        FID_DTC_FOLIO                       enum FontID,  0x121b
        FID_BITSTREAM_FLYER                 enum FontID,  0x321a
        FID_PS_FLYER                        enum FontID,  0x221a
        FID_DTC_FLYER                       enum FontID,  0x121a
        FID_BITSTREAM_FETTE_MIDSCHRIFT      enum FontID,  0x3219
        FID_PS_FETTE_MIDSCHRIFT             enum FontID,  0x2219
        FID_DTC_FETTE_MIDSCHRIFT            enum FontID,  0x1219
        FID_BITSTREAM_FETTE_ENGSCHRIFT      enum FontID,  0x3218
        FID_PS_FETTE_ENGSCHRIFT             enum FontID,  0x2218
        FID_DTC_FETTE_ENGSCHRIFT            enum FontID,  0x1218
        FID_BITSTREAM_ERAS                  enum FontID,  0x3217
        FID_PS_ERAS                         enum FontID,  0x2217
        FID_DTC_ERAS                        enum FontID,  0x1217
        FID_BITSTREAM_DIGI_GROTESK          enum FontID,  0x3216
        FID_PS_DIGI_GROTESK                 enum FontID,  0x2216
        FID_DTC_DIGI_GROTESK                enum FontID,  0x1216
        FID_BITSTREAM_CORINTHIAN            enum FontID,  0x3215
        FID_PS_CORINTHIAN                   enum FontID,  0x2215
        FID_DTC_CORINTHIAN                  enum FontID,  0x1215
        FID_BITSTREAM_COMPACTA              enum FontID,  0x3214
        FID_PS_COMPACTA                     enum FontID,  0x2214
        FID_DTC_COMPACTA                    enum FontID,  0x1214
        FID_BITSTREAM_CLEARFACE_GOTHIC      enum FontID,  0x3213
        FID_PS_CLEARFACE_GOTHIC             enum FontID,  0x2213
        FID_DTC_CLEARFACE_GOTHIC            enum FontID,  0x1213
        FID_BITSTREAM_OPTIMA                enum FontID,  0x3212
        FID_PS_OPTIMA                       enum FontID,  0x2212
        FID_DTC_OPTIMA                      enum FontID,  0x1212
        FID_BITSTREAM_CHELMSFORD            enum FontID,  0x3211
        FID_PS_CHELMSFORD                   enum FontID,  0x2211
        FID_DTC_CHELMSFORD                  enum FontID,  0x1211
        FID_BITSTREAM_CASTLE                enum FontID,  0x3210
        FID_PS_CASTLE                       enum FontID,  0x2210
        FID_DTC_CASTLE                      enum FontID,  0x1210
        FID_BITSTREAM_BRITANNIC             enum FontID,  0x320f
        FID_PS_BRITANNIC                    enum FontID,  0x220f
        FID_DTC_BRITANNIC                   enum FontID,  0x120f
        FID_BITSTREAM_BERLINER_GROTESK      enum FontID,  0x320e
        FID_PS_BERLINER_GROTESK             enum FontID,  0x220e
        FID_DTC_BERLINER_GROTESK            enum FontID,  0x120e
        FID_BITSTREAM_BENGUIAT_GOTHIC       enum FontID,  0x320d
        FID_PS_BENGUIAT_GOTHIC              enum FontID,  0x220d
        FID_DTC_BENGUIAT_GOTHIC             enum FontID,  0x120d
        FID_BITSTREAM_AVANTE_GARDE          enum FontID,  0x320c
        FID_PS_AVANTE_GARDE                 enum FontID,  0x220c
        FID_DTC_AVANTE_GARDE                enum FontID,  0x120c
        FID_BITSTREAM_ANZEIGEN_GROTESK      enum FontID,  0x320b
        FID_PS_ANZEIGEN_GROTESK             enum FontID,  0x220b
        FID_DTC_ANZEIGEN_GROTESK            enum FontID,  0x120b
        FID_BITSTREAM_ANTIQUE_OLIVE         enum FontID,  0x320a
        FID_PS_ANTIQUE_OLIVE                enum FontID,  0x220a
        FID_DTC_ANTIQUE_OLIVE               enum FontID,  0x120a
        FID_BITSTREAM_ALTERNATE_GOTHIC      enum FontID,  0x3209
        FID_PS_ALTERNATE_GOTHIC             enum FontID,  0x2209
        FID_DTC_ALTERNATE_GOTHIC            enum FontID,  0x1209
        FID_BITSTREAM_AKZIDENZ_GROTESK_BUCH enum FontID,  0x3208
        FID_PS_AKZIDENZ_GROTESK_BUCH        enum FontID,  0x2208
        FID_DTC_AKZIDENZ_GROTESK_BUCH       enum FontID,  0x1208
        FID_BITSTREAM_AKZIDENZ_GROTESK      enum FontID,  0x3207
        FID_PS_AKZIDENZ_GROTESK             enum FontID,  0x2207
        FID_DTC_AKZIDENZ_GROTESK            enum FontID,  0x1207
        FID_BITSTREAM_AVENIR                enum FontID,  0x3206
        FID_PS_AVENIR                       enum FontID,  0x2206
        FID_DTC_AVENIR                      enum FontID,  0x1206
        FID_BITSTREAM_UNIVERS               enum FontID,  0x3205
        FID_PS_UNIVERS                      enum FontID,  0x2205
        FID_DTC_UNIVERS                     enum FontID,  0x1205
        FID_BITSTREAM_FRANKLIN_GOTHIC       enum FontID,  0x3204
        FID_PS_FRANKLIN_GOTHIC              enum FontID,  0x2204
        FID_DTC_FRANKLIN_GOTHIC             enum FontID,  0x1204
        FID_BITSTREAM_ANGRO                 enum FontID,  0x3203
        FID_PS_ANGRO                        enum FontID,  0x2203
        FID_DTC_ANGRO                       enum FontID,  0x1203
        FID_BITSTREAM_EUROSTILE             enum FontID,  0x3202
        FID_PS_EUROSTILE                    enum FontID,  0x2202
        FID_DTC_EUROSTILE                   enum FontID,  0x1202
        FID_BITSTREAM_FRUTIGER              enum FontID,  0x3201
        FID_PS_FRUTIGER                     enum FontID,  0x2201
        FID_DTC_FRUTIGER                    enum FontID,  0x1201
        FID_BITSTREAM_URW_SANS              enum FontID,  0x3200
        FID_PS_URW_SANS                     enum FontID,  0x2200
        FID_DTC_URW_SANS                    enum FontID,  0x1200
        FID_BITSTREAM_GALLIARD_ROMAN_ITALIC enum FontID,  0x307e
        FID_PS_GALLIARD_ROMAN_ITALIC        enum FontID,  0x207e
        FID_DTC_GALLIARD_ROMAN_ITALIC       enum FontID,  0x107e
        FID_BITSTREAM_GRANJON               enum FontID,  0x307d
        FID_PS_GRANJON                      enum FontID,  0x207d
        FID_DTC_GRANJON                     enum FontID,  0x107d
        FID_BITSTREAM_GARTH_GRAPHIC         enum FontID,  0x307c
        FID_PS_GARTH_GRAPHIC                enum FontID,  0x207c
        FID_DTC_GARTH_GRAPHIC               enum FontID,  0x107c
        FID_BITSTREAM_BAUER_BODONI          enum FontID,  0x307b
        FID_PS_BAUER_BODONI                 enum FontID,  0x207b
        FID_DTC_BAUER_BODONI                enum FontID,  0x107b
        FID_BITSTREAM_BELWE                 enum FontID,  0x307a
        FID_PS_BELWE                        enum FontID,  0x207a
        FID_DTC_BELWE                       enum FontID,  0x107a
        FID_BITSTREAM_CHARLEMAGNE           enum FontID,  0x3079
        FID_PS_CHARLEMAGNE                  enum FontID,  0x2079
        FID_DTC_CHARLEMAGNE                 enum FontID,  0x1079
        FID_BITSTREAM_TRAJAN                enum FontID,  0x3078
        FID_PS_TRAJAN                       enum FontID,  0x2078
        FID_DTC_TRAJAN                      enum FontID,  0x1078
        FID_BITSTREAM_ADOBE_GARAMOND        enum FontID,  0x3077
        FID_PS_ADOBE_GARAMOND               enum FontID,  0x2077
        FID_DTC_ADOBE_GARAMOND              enum FontID,  0x1077
        FID_BITSTREAM_ZAPF_INTERNATIONAL    enum FontID,  0x3076
        FID_PS_ZAPF_INTERNATIONAL           enum FontID,  0x2076
        FID_DTC_ZAPF_INTERNATIONAL          enum FontID,  0x1076
        FID_BITSTREAM_ZAPF_BOOK             enum FontID,  0x3075
        FID_PS_ZAPF_BOOK                    enum FontID,  0x2075
        FID_DTC_ZAPF_BOOK                   enum FontID,  0x1075
        FID_BITSTREAM_WORCESTER_ROUND       enum FontID,  0x3074
        FID_PS_WORCESTER_ROUND              enum FontID,  0x2074
        FID_DTC_WORCESTER_ROUND             enum FontID,  0x1074
        FID_BITSTREAM_WINDSOR               enum FontID,  0x3073
        FID_PS_WINDSOR                      enum FontID,  0x2073
        FID_DTC_WINDSOR                     enum FontID,  0x1073
        FID_BITSTREAM_WEISS                 enum FontID,  0x3072
        FID_PS_WEISS                        enum FontID,  0x2072
        FID_DTC_WEISS                       enum FontID,  0x1072
        FID_BITSTREAM_WEIDEMANN             enum FontID,  0x3071
        FID_PS_WEIDEMANN                    enum FontID,  0x2071
        FID_DTC_WEIDEMANN                   enum FontID,  0x1071
        FID_BITSTREAM_WALBAUM               enum FontID,  0x3070
        FID_PS_WALBAUM                      enum FontID,  0x2070
        FID_DTC_WALBAUM                     enum FontID,  0x1070
        FID_BITSTREAM_VOLTA                 enum FontID,  0x306f
        FID_PS_VOLTA                        enum FontID,  0x206f
        FID_DTC_VOLTA                       enum FontID,  0x106f
        FID_BITSTREAM_VENDOME               enum FontID,  0x306e
        FID_PS_VENDOME                      enum FontID,  0x206e
        FID_DTC_VENDOME                     enum FontID,  0x106e
        FID_BITSTREAM_VELJOVIC              enum FontID,  0x306d
        FID_PS_VELJOVIC                     enum FontID,  0x206d
        FID_DTC_VELJOVIC                    enum FontID,  0x106d
        FID_BITSTREAM_ADOBE_UTOPIA          enum FontID,  0x306c
        FID_PS_ADOBE_UTOPIA                 enum FontID,  0x206c
        FID_DTC_ADOBE_UTOPIA                enum FontID,  0x106c
        FID_BITSTREAM_USHERWOOD             enum FontID,  0x306b
        FID_PS_USHERWOOD                    enum FontID,  0x206b
        FID_DTC_USHERWOOD                   enum FontID,  0x106b
        FID_BITSTREAM_URW_ANTIQUA           enum FontID,  0x306a
        FID_PS_URW_ANTIQUA                  enum FontID,  0x206a
        FID_DTC_URW_ANTIQUA                 enum FontID,  0x106a
        FID_BITSTREAM_TIMES_NEW_ROMAN       enum FontID,  0x3069
        FID_PS_TIMES_NEW_ROMAN              enum FontID,  0x2069
        FID_DTC_TIMES_NEW_ROMAN             enum FontID,  0x1069
        FID_BITSTREAM_TIMELESS              enum FontID,  0x3068
        FID_PS_TIMELESS                     enum FontID,  0x2068
        FID_DTC_TIMELESS                    enum FontID,  0x1068
        FID_BITSTREAM_TIFFANY               enum FontID,  0x3067
        FID_PS_TIFFANY                      enum FontID,  0x2067
        FID_DTC_TIFFANY                     enum FontID,  0x1067
        FID_BITSTREAM_TIEPOLO               enum FontID,  0x3066
        FID_PS_TIEPOLO                      enum FontID,  0x2066
        FID_DTC_TIEPOLO                     enum FontID,  0x1066
        FID_BITSTREAM_SWIFT                 enum FontID,  0x3065
        FID_PS_SWIFT                        enum FontID,  0x2065
        FID_DTC_SWIFT                       enum FontID,  0x1065
        FID_BITSTREAM_STYMIE                enum FontID,  0x3064
        FID_PS_STYMIE                       enum FontID,  0x2064
        FID_DTC_STYMIE                      enum FontID,  0x1064
        FID_BITSTREAM_STRATFORD             enum FontID,  0x3063
        FID_PS_STRATFORD                    enum FontID,  0x2063
        FID_DTC_STRATFORD                   enum FontID,  0x1063
        FID_BITSTREAM_STONE_SERIF           enum FontID,  0x3062
        FID_PS_STONE_SERIF                  enum FontID,  0x2062
        FID_DTC_STONE_SERIF                 enum FontID,  0x1062
        FID_BITSTREAM_STONE_INFORMAL        enum FontID,  0x3061
        FID_PS_STONE_INFORMAL               enum FontID,  0x2061
        FID_DTC_STONE_INFORMAL              enum FontID,  0x1061
        FID_BITSTREAM_STEMPEL_SCHNEIDLER    enum FontID,  0x3060
        FID_PS_STEMPEL_SCHNEIDLER           enum FontID,  0x2060
        FID_DTC_STEMPEL_SCHNEIDLER          enum FontID,  0x1060
        FID_BITSTREAM_SOUVENIR              enum FontID,  0x305f
        FID_PS_SOUVENIR                     enum FontID,  0x205f
        FID_DTC_SOUVENIR                    enum FontID,  0x105f
        FID_BITSTREAM_SLIMBACH              enum FontID,  0x305e
        FID_PS_SLIMBACH                     enum FontID,  0x205e
        FID_DTC_SLIMBACH                    enum FontID,  0x105e
        FID_BITSTREAM_SERIFA                enum FontID,  0x305d
        FID_PS_SERIFA                       enum FontID,  0x205d
        FID_DTC_SERIFA                      enum FontID,  0x105d
        FID_BITSTREAM_SABON_ANTIQUA         enum FontID,  0x305c
        FID_PS_SABON_ANTIQUA                enum FontID,  0x205c
        FID_DTC_SABON_ANTIQUA               enum FontID,  0x105c
        FID_BITSTREAM_SABON                 enum FontID,  0x305b
        FID_PS_SABON                        enum FontID,  0x205b
        FID_DTC_SABON                       enum FontID,  0x105b
        FID_BITSTREAM_ROMANA                enum FontID,  0x305a
        FID_PS_ROMANA                       enum FontID,  0x205a
        FID_DTC_ROMANA                      enum FontID,  0x105a
        FID_BITSTREAM_ROCKWELL              enum FontID,  0x3059
        FID_PS_ROCKWELL                     enum FontID,  0x2059
        FID_DTC_ROCKWELL                    enum FontID,  0x1059
        FID_BITSTREAM_RENAULT               enum FontID,  0x3058
        FID_PS_RENAULT                      enum FontID,  0x2058
        FID_DTC_RENAULT                     enum FontID,  0x1058
        FID_BITSTREAM_RALEIGH               enum FontID,  0x3057
        FID_PS_RALEIGH                      enum FontID,  0x2057
        FID_DTC_RALEIGH                     enum FontID,  0x1057
        FID_BITSTREAM_QUORUM                enum FontID,  0x3056
        FID_PS_QUORUM                       enum FontID,  0x2056
        FID_DTC_QUORUM                      enum FontID,  0x1056
        FID_BITSTREAM_PROTEUS               enum FontID,  0x3055
        FID_PS_PROTEUS                      enum FontID,  0x2055
        FID_DTC_PROTEUS                     enum FontID,  0x1055
        FID_BITSTREAM_PLANTIN               enum FontID,  0x3054
        FID_PS_PLANTIN                      enum FontID,  0x2054
        FID_DTC_PLANTIN                     enum FontID,  0x1054
        FID_BITSTREAM_PERPETUA              enum FontID,  0x3053
        FID_PS_PERPETUA                     enum FontID,  0x2053
        FID_DTC_PERPETUA                    enum FontID,  0x1053
        FID_BITSTREAM_PACELLA               enum FontID,  0x3052
        FID_PS_PACELLA                      enum FontID,  0x2052
        FID_DTC_PACELLA                     enum FontID,  0x1052
        FID_BITSTREAM_NOVARESE              enum FontID,  0x3051
        FID_PS_NOVARESE                     enum FontID,  0x2051
        FID_DTC_NOVARESE                    enum FontID,  0x1051
        FID_BITSTREAM_NIMROD                enum FontID,  0x3050
        FID_PS_NIMROD                       enum FontID,  0x2050
        FID_DTC_NIMROD                      enum FontID,  0x1050
        FID_BITSTREAM_NIKIS                 enum FontID,  0x304f
        FID_PS_NIKIS                        enum FontID,  0x204f
        FID_DTC_NIKIS                       enum FontID,  0x104f
        FID_BITSTREAM_NAPOLEAN              enum FontID,  0x304e
        FID_PS_NAPOLEAN                     enum FontID,  0x204e
        FID_DTC_NAPOLEAN                    enum FontID,  0x104e
        FID_BITSTREAM_MODERN_NO_216         enum FontID,  0x304d
        FID_PS_MODERN_NO_216                enum FontID,  0x204d
        FID_DTC_MODERN_NO_216               enum FontID,  0x104d
        FID_BITSTREAM_MODERN                enum FontID,  0x304c
        FID_PS_MODERN                       enum FontID,  0x204c
        FID_DTC_MODERN                      enum FontID,  0x104c
        FID_BITSTREAM_MINISTER              enum FontID,  0x304b
        FID_PS_MINISTER                     enum FontID,  0x204b
        FID_DTC_MINISTER                    enum FontID,  0x104b
        FID_BITSTREAM_MESSIDOR              enum FontID,  0x304a
        FID_PS_MESSIDOR                     enum FontID,  0x204a
        FID_DTC_MESSIDOR                    enum FontID,  0x104a
        FID_BITSTREAM_MERIDIEN              enum FontID,  0x3049
        FID_PS_MERIDIEN                     enum FontID,  0x2049
        FID_DTC_MERIDIEN                    enum FontID,  0x1049
        FID_BITSTREAM_MEMPHIS               enum FontID,  0x3048
        FID_PS_MEMPHIS                      enum FontID,  0x2048
        FID_DTC_MEMPHIS                     enum FontID,  0x1048
        FID_BITSTREAM_MELIOR                enum FontID,  0x3047
        FID_PS_MELIOR                       enum FontID,  0x2047
        FID_DTC_MELIOR                      enum FontID,  0x1047
        FID_BITSTREAM_MARCONI               enum FontID,  0x3046
        FID_PS_MARCONI                      enum FontID,  0x2046
        FID_DTC_MARCONI                     enum FontID,  0x1046
        FID_BITSTREAM_MAGNUS                enum FontID,  0x3045
        FID_PS_MAGNUS                       enum FontID,  0x2045
        FID_DTC_MAGNUS                      enum FontID,  0x1045
        FID_BITSTREAM_MAGNA                 enum FontID,  0x3044
        FID_PS_MAGNA                        enum FontID,  0x2044
        FID_DTC_MAGNA                       enum FontID,  0x1044
        FID_BITSTREAM_MADISON               enum FontID,  0x3043
        FID_PS_MADISON                      enum FontID,  0x2043
        FID_DTC_MADISON                     enum FontID,  0x1043
        FID_BITSTREAM_LUCIDA                enum FontID,  0x3042
        FID_PS_LUCIDA                       enum FontID,  0x2042
        FID_DTC_LUCIDA                      enum FontID,  0x1042
        FID_BITSTREAM_LUBALIN_GRAPH         enum FontID,  0x3041
        FID_PS_LUBALIN_GRAPH                enum FontID,  0x2041
        FID_DTC_LUBALIN_GRAPH               enum FontID,  0x1041
        FID_BITSTREAM_LIFE                  enum FontID,  0x3040
        FID_PS_LIFE                         enum FontID,  0x2040
        FID_DTC_LIFE                        enum FontID,  0x1040
        FID_BITSTREAM_LEAWOOD               enum FontID,  0x303f
        FID_PS_LEAWOOD                      enum FontID,  0x203f
        FID_DTC_LEAWOOD                     enum FontID,  0x103f
        FID_BITSTREAM_KORINNA               enum FontID,  0x303e
        FID_PS_KORINNA                      enum FontID,  0x203e
        FID_DTC_KORINNA                     enum FontID,  0x103e
        FID_BITSTREAM_JENSON_OLD_STYLE      enum FontID,  0x303d
        FID_PS_JENSON_OLD_STYLE             enum FontID,  0x203d
        FID_DTC_JENSON_OLD_STYLE            enum FontID,  0x103d
        FID_BITSTREAM_JANSON                enum FontID,  0x303c
        FID_PS_JANSON                       enum FontID,  0x203c
        FID_DTC_JANSON                      enum FontID,  0x103c
        FID_BITSTREAM_JAMILLE               enum FontID,  0x303b
        FID_PS_JAMILLE                      enum FontID,  0x203b
        FID_DTC_JAMILLE                     enum FontID,  0x103b
        FID_BITSTREAM_ITALIA                enum FontID,  0x303a
        FID_PS_ITALIA                       enum FontID,  0x203a
        FID_DTC_ITALIA                      enum FontID,  0x103a
        FID_BITSTREAM_IMPRESSUM             enum FontID,  0x3039
        FID_PS_IMPRESSUM                    enum FontID,  0x2039
        FID_DTC_IMPRESSUM                   enum FontID,  0x1039
        FID_BITSTREAM_HOLLANDER             enum FontID,  0x3038
        FID_PS_HOLLANDER                    enum FontID,  0x2038
        FID_DTC_HOLLANDER                   enum FontID,  0x1038
        FID_BITSTREAM_HIROSHIGE             enum FontID,  0x3037
        FID_PS_HIROSHIGE                    enum FontID,  0x2037
        FID_DTC_HIROSHIGE                   enum FontID,  0x1037
        FID_BITSTREAM_HAWTHORN              enum FontID,  0x3036
        FID_PS_HAWTHORN                     enum FontID,  0x2036
        FID_DTC_HAWTHORN                    enum FontID,  0x1036
        FID_BITSTREAM_GOUDY                 enum FontID,  0x3035
        FID_PS_GOUDY                        enum FontID,  0x2035
        FID_DTC_GOUDY                       enum FontID,  0x1035
        FID_BITSTREAM_GAMMA                 enum FontID,  0x3034
        FID_PS_GAMMA                        enum FontID,  0x2034
        FID_DTC_GAMMA                       enum FontID,  0x1034
        FID_BITSTREAM_GALLIARD              enum FontID,  0x3033
        FID_PS_GALLIARD                     enum FontID,  0x2033
        FID_DTC_GALLIARD                    enum FontID,  0x1033
        FID_BITSTREAM_FRIZ_QUADRATA         enum FontID,  0x3032
        FID_PS_FRIZ_QUADRATA                enum FontID,  0x2032
        FID_DTC_FRIZ_QUADRATA               enum FontID,  0x1032
        FID_BITSTREAM_FENICE                enum FontID,  0x3031
        FID_PS_FENICE                       enum FontID,  0x2031
        FID_DTC_FENICE                      enum FontID,  0x1031
        FID_BITSTREAM_EXCELSIOR             enum FontID,  0x3030
        FID_PS_EXCELSIOR                    enum FontID,  0x2030
        FID_DTC_EXCELSIOR                   enum FontID,  0x1030
        FID_BITSTREAM_ESPRIT                enum FontID,  0x302f
        FID_PS_ESPRIT                       enum FontID,  0x202f
        FID_DTC_ESPRIT                      enum FontID,  0x102f
        FID_BITSTREAM_ELAN                  enum FontID,  0x302e
        FID_PS_ELAN                         enum FontID,  0x202e
        FID_DTC_ELAN                        enum FontID,  0x102e
        FID_BITSTREAM_EGYPTIENNE            enum FontID,  0x302d
        FID_PS_EGYPTIENNE                   enum FontID,  0x202d
        FID_DTC_EGYPTIENNE                  enum FontID,  0x102d
        FID_BITSTREAM_EGIZIO                enum FontID,  0x302c
        FID_PS_EGIZIO                       enum FontID,  0x202c
        FID_DTC_EGIZIO                      enum FontID,  0x102c
        FID_BITSTREAM_EDWARDIAN             enum FontID,  0x302b
        FID_PS_EDWARDIAN                    enum FontID,  0x202b
        FID_DTC_EDWARDIAN                   enum FontID,  0x102b
        FID_BITSTREAM_EDISON                enum FontID,  0x302a
        FID_PS_EDISON                       enum FontID,  0x202a
        FID_DTC_EDISON                      enum FontID,  0x102a
        FID_BITSTREAM_DIGI_ANTIQUA          enum FontID,  0x3029
        FID_PS_DIGI_ANTIQUA                 enum FontID,  0x2029
        FID_DTC_DIGI_ANTIQUA                enum FontID,  0x1029
        FID_BITSTREAM_DEMOS                 enum FontID,  0x3028
        FID_PS_DEMOS                        enum FontID,  0x2028
        FID_DTC_DEMOS                       enum FontID,  0x1028
        FID_BITSTREAM_CUSHING               enum FontID,  0x3027
        FID_PS_CUSHING                      enum FontID,  0x2027
        FID_DTC_CUSHING                     enum FontID,  0x1027
        FID_BITSTREAM_CORONA                enum FontID,  0x3026
        FID_PS_CORONA                       enum FontID,  0x2026
        FID_DTC_CORONA                      enum FontID,  0x1026
        FID_BITSTREAM_CONGRESS              enum FontID,  0x3025
        FID_PS_CONGRESS                     enum FontID,  0x2025
        FID_DTC_CONGRESS                    enum FontID,  0x1025
        FID_BITSTREAM_CONCORDE_NOVA         enum FontID,  0x3024
        FID_PS_CONCORDE_NOVA                enum FontID,  0x2024
        FID_DTC_CONCORDE_NOVA               enum FontID,  0x1024
        FID_BITSTREAM_CONCORDE              enum FontID,  0x3023
        FID_PS_CONCORDE                     enum FontID,  0x2023
        FID_DTC_CONCORDE                    enum FontID,  0x1023
        FID_BITSTREAM_CLEARFACE             enum FontID,  0x3022
        FID_PS_CLEARFACE                    enum FontID,  0x2022
        FID_DTC_CLEARFACE                   enum FontID,  0x1022
        FID_BITSTREAM_CLARENDON             enum FontID,  0x3021
        FID_PS_CLARENDON                    enum FontID,  0x2021
        FID_DTC_CLARENDON                   enum FontID,  0x1021
        FID_BITSTREAM_CHELTENHAM            enum FontID,  0x3020
        FID_PS_CHELTENHAM                   enum FontID,  0x2020
        FID_DTC_CHELTENHAM                  enum FontID,  0x1020
        FID_BITSTREAM_CENTURY_OLD_STYLE     enum FontID,  0x301f
        FID_PS_CENTURY_OLD_STYLE            enum FontID,  0x201f
        FID_DTC_CENTURY_OLD_STYLE           enum FontID,  0x101f
        FID_BITSTREAM_CENTURY               enum FontID,  0x301e
        FID_PS_CENTURY                      enum FontID,  0x201e
        FID_DTC_CENTURY                     enum FontID,  0x101e
        FID_BITSTREAM_CENTENNIAL            enum FontID,  0x301d
        FID_PS_CENTENNIAL                   enum FontID,  0x201d
        FID_DTC_CENTENNIAL                  enum FontID,  0x101d
        FID_BITSTREAM_CAXTON                enum FontID,  0x301c
        FID_PS_CAXTON                       enum FontID,  0x201c
        FID_DTC_CAXTON                      enum FontID,  0x101c
        FID_BITSTREAM_ADOBE_CASLON          enum FontID,  0x301b
        FID_PS_ADOBE_CASLON                 enum FontID,  0x201b
        FID_DTC_ADOBE_CASLON                enum FontID,  0x101b
        FID_BITSTREAM_CASLON                enum FontID,  0x301a
        FID_PS_CASLON                       enum FontID,  0x201a
        FID_DTC_CASLON                      enum FontID,  0x101a
        FID_BITSTREAM_CANDIDA               enum FontID,  0x3019
        FID_PS_CANDIDA                      enum FontID,  0x2019
        FID_DTC_CANDIDA                     enum FontID,  0x1019
        FID_BITSTREAM_BOOKMAN               enum FontID,  0x3018
        FID_PS_BOOKMAN                      enum FontID,  0x2018
        FID_DTC_BOOKMAN                     enum FontID,  0x1018
        FID_BITSTREAM_BASKERVILLE_HANDCUT   enum FontID,  0x3017
        FID_PS_BASKERVILLE_HANDCUT          enum FontID,  0x2017
        FID_DTC_BASKERVILLE_HANDCUT         enum FontID,  0x1017
        FID_BITSTREAM_BASKERVILLE           enum FontID,  0x3016
        FID_PS_BASKERVILLE                  enum FontID,  0x2016
        FID_DTC_BASKERVILLE                 enum FontID,  0x1016
        FID_BITSTREAM_BASILIA               enum FontID,  0x3015
        FID_PS_BASILIA                      enum FontID,  0x2015
        FID_DTC_BASILIA                     enum FontID,  0x1015
        FID_BITSTREAM_BARBEDOR              enum FontID,  0x3014
        FID_PS_BARBEDOR                     enum FontID,  0x2014
        FID_DTC_BARBEDOR                    enum FontID,  0x1014
        FID_BITSTREAM_AUREALIA              enum FontID,  0x3013
        FID_PS_AUREALIA                     enum FontID,  0x2013
        FID_DTC_AUREALIA                    enum FontID,  0x1013
        FID_BITSTREAM_NEW_ASTER             enum FontID,  0x3012
        FID_PS_NEW_ASTER                    enum FontID,  0x2012
        FID_DTC_NEW_ASTER                   enum FontID,  0x1012
        FID_BITSTREAM_ASTER                 enum FontID,  0x3011
        FID_PS_ASTER                        enum FontID,  0x2011
        FID_DTC_ASTER                       enum FontID,  0x1011
        FID_BITSTREAM_AMERICANA             enum FontID,  0x3010
        FID_PS_AMERICANA                    enum FontID,  0x2010
        FID_DTC_AMERICANA                   enum FontID,  0x1010
        FID_BITSTREAM_AACHEN                enum FontID,  0x300f
        FID_PS_AACHEN                       enum FontID,  0x200f
        FID_DTC_AACHEN                      enum FontID,  0x100f
        FID_BITSTREAM_NICOLAS_COCHIN        enum FontID,  0x300e
        FID_PS_NICOLAS_COCHIN               enum FontID,  0x200e
        FID_DTC_NICOLAS_COCHIN              enum FontID,  0x100e
        FID_BITSTREAM_COCHIN                enum FontID,  0x300d
        FID_PS_COCHIN                       enum FontID,  0x200d
        FID_DTC_COCHIN                      enum FontID,  0x100d
        FID_BITSTREAM_ALBERTUS              enum FontID,  0x300c
        FID_PS_ALBERTUS                     enum FontID,  0x200c
        FID_DTC_ALBERTUS                    enum FontID,  0x100c
        FID_BITSTREAM_ACCOLADE              enum FontID,  0x300b
        FID_PS_ACCOLADE                     enum FontID,  0x200b
        FID_DTC_ACCOLADE                    enum FontID,  0x100b
        FID_BITSTREAM_PALATINO              enum FontID,  0x300a
        FID_PS_PALATINO                     enum FontID,  0x200a
        FID_DTC_PALATINO                    enum FontID,  0x100a
        FID_BITSTREAM_GOUDY_OLD_STYLE       enum FontID,  0x3009
        FID_PS_GOUDY_OLD_STYLE              enum FontID,  0x2009
        FID_DTC_GOUDY_OLD_STYLE             enum FontID,  0x1009
        FID_BITSTREAM_BERKELEY_OLD_STYLE    enum FontID,  0x3008
        FID_PS_BERKELEY_OLD_STYLE           enum FontID,  0x2008
        FID_DTC_BERKELEY_OLD_STYLE          enum FontID,  0x1008
        FID_BITSTREAM_ARSIS                 enum FontID,  0x3007
        FID_PS_ARSIS                        enum FontID,  0x2007
        FID_DTC_ARSIS                       enum FontID,  0x1007
        FID_BITSTREAM_UNIVERSITY_ROMAN      enum FontID,  0x3006
        FID_PS_UNIVERSITY_ROMAN             enum FontID,  0x2006
        FID_DTC_UNIVERSITY_ROMAN            enum FontID,  0x1006
        FID_BITSTREAM_BEMBO                 enum FontID,  0x3005
        FID_PS_BEMBO                        enum FontID,  0x2005
        FID_DTC_BEMBO                       enum FontID,  0x1005
        FID_BITSTREAM_GARAMOND              enum FontID,  0x3004
        FID_PS_GARAMOND                     enum FontID,  0x2004
        FID_DTC_GARAMOND                    enum FontID,  0x1004
        FID_BITSTREAM_GLYPHA                enum FontID,  0x3003
        FID_PS_GLYPHA                       enum FontID,  0x2003
        FID_DTC_GLYPHA                      enum FontID,  0x1003
        FID_BITSTREAM_BODONI                enum FontID,  0x3002
        FID_PS_BODONI                       enum FontID,  0x2002
        FID_DTC_BODONI                      enum FontID,  0x1002
        FID_BITSTREAM_CENTURY_SCHOOLBOOK    enum FontID,  0x3001
        FID_PS_CENTURY_SCHOOLBOOK           enum FontID,  0x2001
        FID_DTC_CENTURY_SCHOOLBOOK          enum FontID,  0x1001
        FID_BITSTREAM_URW_ROMAN             enum FontID,  0x3000
        FID_PS_TIMES_ROMAN                  enum FontID,  0x2000
        FID_DTC_URW_ROMAN                   enum FontID,  0x1000
        FID_WINDOWS                         enum FontID,  0x0a01
        FID_BISON                           enum FontID,  0x0a00
        FID_LED                             enum FontID,  0x0600
        FID_PMSYSTEM                        enum FontID,  0x0203
        FID_BERKELEY                        enum FontID,  0x0202
        FID_UNIVERSITY                      enum FontID,  0x0201
        FID_CHICAGO                         enum FontID,  0x0200
        FID_ROMA                            enum FontID,  0x0001

The lower twelve bits for any particular font face are the same. (For example 
SCHOOLBOOK faces end with 001. The first 4 bits define the particular 
maker. Thus, each particular face may have up to 16 different makers.

The only exceptions to this naming scheme are the printer and bitstream 
fonts, and FID_INVALID, which is a special case and is set to all zeros.

**Library:** fontID.def

----------
#### FontIDRecord
    FontIDRecord            record
        FIDR_maker      :4
        FIDR_ID         :12
    FontIDRecord            end

**Library:** font.def

----------
#### FontMaker
    FontMaker       etype       word, 0, FID_MAKER_DIVISIONS
        FM_BITMAP           enum FontMaker
        FM_NIMBUSQ          enum FontMaker
        FM_ADOBE            enum FontMaker
        FM_BITSTREAM        enum FontMaker
        FM_AGFA             enum FontMaker
        FM_PUBLIC           enum FontMaker, 0xc000
        FM_ATECH            enum FontMaker, 0xd000
        FM_MICROLOGIC       enum FontMaker, 0xe000
        FM_PRINTER          enum FontMaker, 0xf000

**Library:** fontID.def

----------
#### FontMap
    FontMap etype   byte, 0
        FM_EXACT        enum FontMap, 0
        FM_DONT_USE     enum FontMap, 0xff

**Library:** fontID.def

----------
#### FontOrientation
    FontOrientation     etype   byte
        FO_NORMAL       enum FontOrientation    ; normal straight up & down font
        FO_LANDSCAPE    enum FontOrientation    ; rotated 90 degrees.

**Library:** font.def

----------
#### FontPitch
    FontPitch       etype       byte
        FP_PROPORTIONAL     enum FontPitch  ; proportional font.
        FP_FIXED            enum FontPitch  ; Fixed pitch font.

**Library:** font.def

----------
#### FontSource
    FontSource      etype   byte
        FS_BITMAP       enum FontSource     ; bitmap data
        FS_OUTLINE      enum FontSource     ; outline data

**Library:** font.def

----------
#### FontUseful
    FontUseful      etype   byte
        FU_NOT_USEFUL   enum FontUseful     ; not useful for menus
        FU_USEFUL       enum FontUseful     ; useful for menus

**Library:** font.def

----------
#### FontWeight
    FontWeight      etype   byte
        FW_MINIMUM      enum FontWeight, 75
        FW_NORMAL       enum FontWeight, 100
        FW_MAXIMUM      enum FontWeight, 125

**Library:** font.def

----------
#### FontWidth
    FontWidth       etype       byte
        FWI_MINIMUM         enum FontWidth, 25
        FWI_NARROW          enum FontWidth, 75
        FWI_CONDENSED       enum FontWidth, 85
        FWI_MEDIUM          enum FontWidth, 100
        FWI_WIDE            enum FontWidth, 125
        FWI_EXPANDED        enum FontWidth, 150
        FWI_MAXIMUM         enum FontWidth, 200

This type defines the width of the font as a percentage of its normal width.

**Library:** font.def

----------
#### FormatArrayHeader
    FormatArrayHeader       struc
        FAH_signature           word
        FAH_numFormatEntries    word    ; format array entries that have
                                        ; been allocated (possibly free)
        FAH_numUserDefEntries   word
        FAH_formatArrayEnd      word    ;offset to end of format array
    FormatArrayHeader       ends

**Library:** math.def

----------
#### FormatEntry
    FormatEntry     struc
        FE_params           FormatParams
        FE_listEntryNumber  word        ; list entry number
                                        ; This will allow us to get the right
                                        ; entry given the list entry
        FE_used             byte        ; boolean, 0 if entry is free
        FE_sig              word        ; signature for EC purposes
    FormatEntry     ends

**Library:** math.def

----------
#### FormatError
    FormatError     etype   word
        FMT_DONE                            enum FormatError, 0
        FMT_READY                           enum FormatError
        FMT_RUNNING                         enum FormatError
        FMT_DRIVE_NOT_READY                 enum FormatError
        FMT_ERR_WRITING_BOOT                enum FormatError
        FMT_ERR_WRITING_ROOT_DIR            enum FormatError
        FMT_ERR_WRITING_FAT                 enum FormatError
        FMT_ABORTED                         enum FormatError
        FMT_SET_VOLUME_NAME_ERR             enum FormatError
        FMT_CANNOT_FORMAT_FIXED_DISKS_IN_CUR_RELEASE enum FormatError
        FMT_BAD_PARTITION_TABLE             enum FormatError ;fixed disk
        FMT_ERR_READING_PARTITION_TABLE     enum FormatError ;fixed disk
        FMT_ERR_NO_PARTITION_FOUND          enum FormatError ;fixed disk
        FMT_ERR_MULTIPLE_PRIMARY_PARTITIONS enum FormatError ;fixed disk
        FMT_ERR_NO_EXTENDED_PARTITION_FOUND enum FormatError ;fixed disk
        FMT_ERR_CANNOT_ALLOC_SECTOR_BUFFER  enum FormatError
        FMT_ERR_DISK_IS_IN_USE              enum FormatError
        FMT_ERR_WRITE_PROTECTED             enum FormatError
        FMT_ERR_DRIVE_CANNOT_SUPPORT_GIVEN_FORMAT enum FormatError
        FMT_ERR_INVALID_DRIVE_SPECIFIED     enum FormatError
        FMT_ERR_DRIVE_CANNOT_BE_FORMATTED   enum FormatError
        FMT_ERR_DISK_UNAVAILABLE            enum FormatError
        FMT_ERR_CANNOT_FORMAT_TRACK         enum FormatError ; catch-all

**Library:** disk.def

----------
#### FormatInfoStruc
    FormatInfoStruc     struc
        FIS_signature                   word
        FIS_userDefFmtArrayFileHan      word
        FIS_userDefFmtArrayBlkHan       word
        FIS_childBlk                    word
        FIS_chooseFmtListChunk          word
        FIS_features                    FFCFeatures
        FIS_editFlag                    byte
        FIS_curSelection                word
        FIS_curToken                    word
        FIS_curParams                   FormatParams
    FormatInfoStruc     ends

This structure is passed as a block to the Float Format create and edit code. 
This structure is used by the Float Format controller to specify the specific 
format to act on.

When passed to routines, *FIS_userDefFmtArrayFileHan* and 
*FIS_userDefFmtArrayBlkHan* must be properly set up to hold the VM file and 
block handles of the user-defined format array.

*FIS_signature* stores internal signatures used by error-checking code.

*FIS_userDefFmtArrayFileHan* stores the VM file handle of the user-defined 
format array. This must be properly set up even if no user-defined formats are 
to be used.

*FIS_userDefFmtArrayBlkHan* stores the VM block handle of the user-defined 
format array. This must be properly set up even if no user-defined formats are 
to be used.

*FIS_childBlk* and *FIS_chooseFmtListChunk* store the optr to the dynamic list 
object within the Float Format controller.

*FIS_features* stores the features list of the Float Format controller.

*FIS_editFlag* stores a non-zero value if the format entry is currently being 
used. If this entry is later freed, this edit flag is set to zero to indicate that 
this entry is available for other formats.

*FIS_curSelection* stores the current selection of the Float Format controller's 
dynamic list.

*FIS_curToken* stores the current **FormatIdType** of the format entry. In 
many cases, this token is passed in this structure and *FIS_curParams* is filled 
in with the matching **FormatParams**.

*FIS_curParams* stores the current **FormatParams** of the format entry.

**Library:** math.def

----------
#### FormatNameParams
    FormatNameParams        struct
        FNP_listEntry   word                ; the entry number in the defined 
                                            ; list
        FNP_textLength  word                ; length of the format name
        FNP_text        byte FORMAT_NAME_LENGTH dup (?)
        FNP_token       word                ; the token of the format
        align           word
    FormatNameParams        ends

**Library:** math.def

----------
#### FormatOption
    FormatOption        record
                                :2
        FO_COMMA                :1
        FO_PCT                  :1
        FO_LEAD_ZERO            :1
        FO_TRAIL_ZERO           :1
        FO_HEADER_SIGN_POS      :1
        FO_TRAILER_SIGN_POS     :1
    FormatOption        end

**Library:** math.def

----------
#### FormatParams
    FormatParams        struc
        FP_params           FloatFloatToAsciiParams_Union
        FP_formatName       char FORMAT_NAME_LENGTH+1 dup (?)
        FP_nameHan          word
        FP_nameOff          word
        FP_listEntryNum     word
        FP_signature        word                    ; internal
    FormatParams        ends

This structure stores the formatting parameters used by the Float Format 
controller to format FP numbers into text.

*FP_params* stores the **FloatFloatToAscii** parameters to use when 
formatting the FP number into text.

*FP_formatName* stores the text name of the format entry to display in the 
Float Format controller's dynamic list. This text is stored at the optr defined 
by *FP_nameHan* and *FP_nameOff*.

*FP_nameHan* and *FP_nameOff* store the optr to the text strings where the 
format names are kept.

*FP_listEntryNumber* stores the zero-based position of the format entry 
within the dynamic list.

**Library:** parse.def

----------
#### FormatParameters
    FormatParameters        struct
        FP_common       CommonParameters <>
        FP_nChars       word                ; Number of bytes left in the buffer.
    FormatParameters        ends

**Library:** parse.def

----------
#### FRSPFlags
    FRSPFlags       record
                                        :14
        FRSPF_ADD_DRIVE_NAME            :1
        FRSPF_RETURN_FIRST_DIR          :1
    FRSPFlags       end

FRSPF_ADD_DRIVE_NAME  
Set if **FileResolveStandardPath** should prepend name of the drive in 
which the file or directory was found to the path.

FRSPF_RETURN_FIRST_DIR  
Set if **FileResolveStandardPath** should not check to see whether the 
passed path actually exists, but instead assume it exists in the first existing 
directory along the standard path.

**Library:** file.def

----------
#### FTVMCGrab
    FTVMCGrab       struct
        FTVMC_OD            optr
        FTVMC_flags         MetaAlterFTVMCExclFlags
    FTVMCGrab       ends

This structure is a variation on the basic **MetaAlterFTVMCExclFlags** 
record, adding the optr of the Focus/Target/Model hierarchical grab.

**Library:** uiInputC.def

----------
#### FunctionID
    FunctionID      etype   word, 0, 2
        FUNCTION_ID_ABS                 enum FunctionID
        FUNCTION_ID_ACOS                enum FunctionID
        FUNCTION_ID_ACOSH               enum FunctionID
        FUNCTION_ID_AND                 enum FunctionID
        FUNCTION_ID_ASIN                enum FunctionID
        FUNCTION_ID_ASINH               enum FunctionID
        FUNCTION_ID_ATAN                enum FunctionID
        FUNCTION_ID_ATAN2               enum FunctionID
        FUNCTION_ID_ATANH               enum FunctionID
        FUNCTION_ID_AVG                 enum FunctionID
        FUNCTION_ID_CHAR                enum FunctionID
        FUNCTION_ID_CHOOSE              enum FunctionID
        FUNCTION_ID_CLEAN               enum FunctionID
        FUNCTION_ID_CODE                enum FunctionID
        FUNCTION_ID_COLS                enum FunctionID
        FUNCTION_ID_COS                 enum FunctionID
        FUNCTION_ID_COSH                enum FunctionID
        FUNCTION_ID_COUNT               enum FunctionID
        FUNCTION_ID_CTERM               enum FunctionID
        FUNCTION_ID_DATE                enum FunctionID
        FUNCTION_ID_DATEVALUE           enum FunctionID
        FUNCTION_ID_DAY                 enum FunctionID
        FUNCTION_ID_DDB                 enum FunctionID
        FUNCTION_ID_ERR                 enum FunctionID
        FUNCTION_ID_EXACT               enum FunctionID
        FUNCTION_ID_EXP                 enum FunctionID
        FUNCTION_ID_FACT                enum FunctionID
        FUNCTION_ID_FALSE               enum FunctionID
        FUNCTION_ID_FIND                enum FunctionID
        FUNCTION_ID_FV                  enum FunctionID
        FUNCTION_ID_HLOOKUP             enum FunctionID
        FUNCTION_ID_HOUR                enum FunctionID
        FUNCTION_ID_IF                  enum FunctionID
        FUNCTION_ID_INDEX               enum FunctionID
        FUNCTION_ID_INT                 enum FunctionID
        FUNCTION_ID_IRR                 enum FunctionID
        FUNCTION_ID_ISERR               enum FunctionID
        FUNCTION_ID_ISNUMBER            enum FunctionID
        FUNCTION_ID_ISSTRING            enum FunctionID
        FUNCTION_ID_LEFT                enum FunctionID
        FUNCTION_ID_LENGTH              enum FunctionID
        FUNCTION_ID_LN                  enum FunctionID
        FUNCTION_ID_LOG                 enum FunctionID
        FUNCTION_ID_LOWER               enum FunctionID
        FUNCTION_ID_MAX                 enum FunctionID
        FUNCTION_ID_MID                 enum FunctionID
        FUNCTION_ID_MIN                 enum FunctionID
        FUNCTION_ID_MINUTE              enum FunctionID
        FUNCTION_ID_MOD                 enum FunctionID
        FUNCTION_ID_MONTH               enum FunctionID
        FUNCTION_ID_N                   enum FunctionID
        FUNCTION_ID_NA                  enum FunctionID
        FUNCTION_ID_NOW                 enum FunctionID
        FUNCTION_ID_NPV                 enum FunctionID
        FUNCTION_ID_OR                  enum FunctionID
        FUNCTION_ID_PI                  enum FunctionID
        FUNCTION_ID_PMT                 enum FunctionID
        FUNCTION_ID_PRODUCT             enum FunctionID
        FUNCTION_ID_PROPER              enum FunctionID
        FUNCTION_ID_PV                  enum FunctionID
        FUNCTION_ID_RANDOM_N            enum FunctionID
        FUNCTION_ID_RANDOM              enum FunctionID
        FUNCTION_ID_RATE                enum FunctionID
        FUNCTION_ID_REPEAT              enum FunctionID
        FUNCTION_ID_REPLACE             enum FunctionID
        FUNCTION_ID_RIGHT               enum FunctionID
        FUNCTION_ID_ROUND               enum FunctionID
        FUNCTION_ID_ROWS                enum FunctionID
        FUNCTION_ID_SECOND              enum FunctionID
        FUNCTION_ID_SIN                 enum FunctionID
        FUNCTION_ID_SINH                enum FunctionID
        FUNCTION_ID_SLN                 enum FunctionID
        FUNCTION_ID_SQRT                enum FunctionID
        FUNCTION_ID_STD                 enum FunctionID
        FUNCTION_ID_STDP                enum FunctionID
        FUNCTION_ID_STRING              enum FunctionID
        FUNCTION_ID_SUM                 enum FunctionID
        FUNCTION_ID_SYD                 enum FunctionID
        FUNCTION_ID_TAN                 enum FunctionID
        FUNCTION_ID_TANH                enum FunctionID
        FUNCTION_ID_TERM                enum FunctionID
        FUNCTION_ID_TIME                enum FunctionID
        FUNCTION_ID_TIMEVALUE           enum FunctionID
        FUNCTION_ID_TODAY               enum FunctionID
        FUNCTION_ID_TRIM                enum FunctionID
        FUNCTION_ID_TRUE                enum FunctionID
        FUNCTION_ID_TRUNC               enum FunctionID
        FUNCTION_ID_UPPER               enum FunctionID
        FUNCTION_ID_VALUE               enum FunctionID
        FUNCTION_ID_VAR                 enum FunctionID
        FUNCTION_ID_VARP                enum FunctionID
        FUNCTION_ID_VLOOKUP             enum FunctionID
        FUNCTION_ID_WEEKDAY             enum FunctionID
        FUNCTION_ID_YEAR                enum FunctionID
        FUNCTION_ID_FILENAME            enum FunctionID
        FUNCTION_ID_PAGE                enum FunctionID
        FUNCTION_ID_PAGES               enum FunctionID
        FUNCTION_ID_DEGREES             enum FunctionID
        FUNCTION_ID_RADIANS             enum FunctionID
        ;
        ; External functions (defined by the application) start here.
        ;
        FUNCTION_ID_FIRST_EXTERNAL_FUNCTION enum    FunctionID, 0x8000

**Library:** parse.def

----------
#### FunctionType
    FunctionType        record
            :7
        FT_PRINT            :1
        FT_TRIGONOMETRIC    :1
        FT_LOGICAL          :1
        FT_STATISTICAL      :1
        FT_STRING           :1
        FT_TIME_DATE        :1
        FT_FINANCIAL        :1
        FT_MATH             :1
        FT_INFORMATION      :1
    FunctionType        end

**Library:** parse.def

[Structures A-C](asmstrac.md) <-- [Table of Contents](../asmref.md) &nbsp;&nbsp; --> [Structures G-G](asmstrgg.md)