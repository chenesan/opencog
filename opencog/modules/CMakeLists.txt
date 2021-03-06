
IF (HAVE_SERVER)
	ADD_SUBDIRECTORY(benchmark)
	IF (HAVE_CYTHON)
		ADD_SUBDIRECTORY(python)
	ENDIF (HAVE_CYTHON)
ENDIF (HAVE_SERVER)

IF (HAVE_REST)
	ADD_SUBDIRECTORY(events)
ENDIF (HAVE_REST)

# ----------------------------------------
# Link-grammar cogserver module

IF (HAVE_GUILE AND HAVE_LINK_GRAMMAR)
	ADD_LIBRARY (LGDictModule SHARED
		LGDictModule
	)

	TARGET_LINK_LIBRARIES (LGDictModule lg-dict server)

	INSTALL (TARGETS LGDictModule
		LIBRARY DESTINATION "lib${LIB_DIR_SUFFIX}/opencog/modules"
	)
ENDIF (HAVE_GUILE AND HAVE_LINK_GRAMMAR)

# Build the query (pattern-matching) cogserver module
ADD_LIBRARY(QueryModule SHARED
	QueryModule.cc
)
TARGET_LINK_LIBRARIES(QueryModule
	${ATOMSPACE_LIBRARIES}
)

INSTALL (TARGETS QueryModule
	LIBRARY DESTINATION "lib${LIB_DIR_SUFFIX}/opencog/modules"
)
# ----------------------------------------
# The SQL persistence module

IF (HAVE_PERSIST_SQL)
	ADD_LIBRARY (PersistModule SHARED
		PersistModule.cc
	)

	TARGET_LINK_LIBRARIES(PersistModule
		# server  # needed for definition of _ZTIN7opencog7RequestE
		${ATOMSPACE_LIBRARIES}
	)

	INSTALL (TARGETS PersistModule
		LIBRARY DESTINATION "lib${LIB_DIR_SUFFIX}/opencog/modules"
	)
ENDIF (HAVE_PERSIST_SQL)

# ----------------------------------------
# The ZeroMQ persistence module

IF (HAVE_ZMQ AND HAVE_PROTOBUF)
	
	ADD_LIBRARY (PersistZmqModule SHARED
		PersistZmqModule.cc
	)
	
	TARGET_LINK_LIBRARIES(PersistZmqModule
		${ATOMSPACE_LIBRARIES} ${PROTOBUF_LIBRARY}
	)
	
	INSTALL (TARGETS PersistZmqModule
		LIBRARY DESTINATION "lib${LIB_DIR_SUFFIX}/opencog/modules"
	)
	
ENDIF (HAVE_ZMQ AND HAVE_PROTOBUF)

# ----------------------------------------
# Build the RuleEngine cogserver module

ADD_LIBRARY(RuleEngineModule SHARED
	RuleEngineModule.cc
)

TARGET_LINK_LIBRARIES(RuleEngineModule
	${ATOMSPACE_LIBRARIES}
)

INSTALL (TARGETS RuleEngineModule
	LIBRARY DESTINATION "lib${LIB_DIR_SUFFIX}/opencog/modules"
)
# ----------------------------------------
# Surface realization cogserver module

IF (HAVE_GUILE AND HAVE_LINK_GRAMMAR)
	ADD_LIBRARY (SuRealModule SHARED
		SuRealModule
	)

	ADD_DEPENDENCIES (SuRealModule
		nlp_atom_types
	)

	TARGET_LINK_LIBRARIES(SuRealModule
		sureal
	)

	INSTALL (TARGETS SuRealModule
		LIBRARY DESTINATION "lib${LIB_DIR_SUFFIX}/opencog/modules"
	)
ENDIF (HAVE_GUILE AND HAVE_LINK_GRAMMAR)
