CC=gcc
CXX=g++
CFLAGS=-ggdb -fvar-tracking-assignments -Wall -Wextra -std=gnu99 -fsanitize=address
CXXFLAGS=-ggdb -fvar-tracking-assignments -Wall -Wextra --std=gnu++11 -Iextern -fsanitize=address
LDFLAGS=-fsanitize=address

.POSIX:

RAPIDXML_FLAGS:= -Iextern/rapidxml-1.13
SFML_LDFLAGS:=-lsfml-system -lsfml-window -lsfml-graphics -lsfml-audio -lsfgui

CXXFLAGS += $(RAPIDXML_FLAGS)
LDFLAGS += $(SFML_LDFLAGS)

default: .POSIX wasteland

SRC:=\
	wasteland.cpp\
	character.cpp\
	map.cpp\
	utility.cpp\
	object.cpp\
	levelgen.cpp\
	extern/fov.c\
	extern/miniz.c

CXX_SRC=$(filter %.cpp,$(SRC))
C_SRC=$(filter %.c,$(SRC))
OBJ:=$(C_SRC:.c=.o) $(CXX_SRC:.cpp=.o)

wasteland: $(SRC) $(OBJ)
	$(CXX) $(CXXFLAGS) $(LDFLAGS) $(OBJ) -o wasteland
	
clean:
	- rm -f $(OBJ) wasteland
