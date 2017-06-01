SRC=../src
OBJ=../lib

CXXFLAGS+=$(shell root-config --cflags) -Wall -I$(SRC) -ggdb3 -fPIC
LDFLAGS+=$(shell root-config --glibs)  -ggdb3 

LINKD=$(SRC)/TotemEvent_LinkDef

all: $(OBJ) $(OBJ)/libTOTEMdataFormat.so 

$(OBJ)/libTOTEMdataFormat.so: $(OBJ)/T2Event.o $(OBJ)/T1Event.o $(OBJ)/TriggerData.o $(OBJ)/EventMetaData.o $(OBJ)/RPRootDumpDigiInfo.o $(OBJ)/RPEvent.o $(OBJ)/RPRootDumpPattern.o $(OBJ)/RPRootDumpPatternInfo.o $(OBJ)/RPRootDumpReconstructedProton.o $(OBJ)/RPRootDumpReconstructedProtonPair.o $(OBJ)/RPRootDumpTrackInfo.o $(OBJ)/NtupleInfo.o $(OBJ)/eventdictT.o 
	$(CXX) -shared -o $@ $^ $(LDFLAGS) # order important for Ubuntu
	if [ ! -s $(OBJ)/eventdictT_rdict.pcm ] ; then ln -s $(SRC)leventdictT_rdict.pcm $(OBJ)/ ; fi
	@echo ""
	#to load the library to CINT one needs X connection
	@echo ""

$(OBJ)/%.o: $(SRC)/%.cc
	$(CXX) $(CXXFLAGS) -c $< -o $@
	@echo ""

$(SRC)/eventdictT.cc: $(SRC)/T2Event.h $(SRC)/T1Event.h $(SRC)/TriggerData.h $(SRC)/EventMetaData.h $(SRC)/RPRootDumpDigiInfo.h $(SRC)/RPEvent.h $(SRC)/RPRootDumpPattern.h $(SRC)/RPRootDumpPatternInfo.h $(SRC)/RPRootDumpReconstructedProton.h $(SRC)/RPRootDumpReconstructedProtonPair.h $(SRC)/RPRootDumpTrackInfo.h $(SRC)/NtupleInfo.h $(LINKD).h
	rootcint -f $@ -c -p $^
	#compatibility with cmssw
	mv $(LINKD).h $(LINKD)h
	ln -s $(SRC)/eventdictT_rdict.pcm $(OBJ)/
	@echo ""

$(LINKD).h:
	#compatibility with cmssw
	mv $(LINKD)h  $(LINKD).h
	@echo ""

$(OBJ): 
	mkdir -p $(OBJ)
clean:
	rm -rf $(OBJ) $(SRC)/eventdictT.*

