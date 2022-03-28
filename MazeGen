#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>

template < typename T >
std::vector< std::vector< T > > createEmptyMatrix(int numRows, int numColumns) {
    std::vector< std::vector< T > > matrix;
    for (int i = 0; i < numRows; i++) {
        std::vector< T > row;
        for (int j = 0; j < numColumns; j++) {
            row.push_back( T() );
        }
        matrix.push_back(row);
    }
    return matrix;
}


class Cell {
    public:
        bool left;
        bool right;
        bool down;
        bool up;
        bool isPath;

        Cell();  
};

Cell::Cell() {
    left = right = down = up = isPath = false;
}


class Maze {
    public:
        int numRows;
        int numColumns;

        Maze(int rows, int columns);
        void printMaze();

    private:
        std::vector< std::vector < Cell > > mazeMatrix;
        std::vector< int > dy = { 0, 1, 0, -1 };
        std::vector< int > dx = { 1, 0, -1, 0 };

        void generateMaze(std::vector< std::vector< bool > > &isVisited, int row, int column);
        bool findPath(std::vector< std::vector< bool > > &isVisited, int row, int column);
};

Maze::Maze(int rows, int columns) {
    numRows = rows;
    numColumns = columns;

    std::vector< std::vector< bool > > isVisited = createEmptyMatrix< bool >(numRows, numColumns);
    mazeMatrix = createEmptyMatrix< Cell >(numRows, numColumns);

    generateMaze(isVisited, 0, 0);

    isVisited = createEmptyMatrix< bool >(numRows, numColumns);
    findPath(isVisited, 0, 0);
}

void Maze::generateMaze(std::vector< std::vector< bool > > &isVisited, int row, int column) {
    isVisited.at(row).at(column) = true;

    int cnt = 0;    
    while (cnt < 4) {
        bool deadEnd = true;
        for (int dir=0; dir < 4; dir++) {
            int newRow = row + dy.at(dir);
            int newCol = column + dx.at(dir);

            if (newRow >= isVisited.size() || newRow < 0 || newCol >= isVisited.at(0).size() || newCol < 0) continue;
            if (isVisited.at(newRow).at(newCol)) continue;

            deadEnd = false;
            break;
        }
        if (deadEnd) break;

        int dir = rand() % 4;
        int newRow = row + dy.at(dir);
        int newCol = column + dx.at(dir);

        if (newRow >= isVisited.size() || newRow < 0 || newCol >= isVisited.at(0).size() || newCol < 0) continue;
        if (isVisited.at(newRow).at(newCol)) continue;

        if (dir == 0) { //goes right
            mazeMatrix.at(row).at(column).right = true;
            mazeMatrix.at(newRow).at(newCol).left = true;
        }
        if (dir == 1) { //goes down
            mazeMatrix.at(row).at(column).down = true;
            mazeMatrix.at(newRow).at(newCol).up = true;
        }
        if (dir == 2) { //goes left
            mazeMatrix.at(row).at(column).left = true;
            mazeMatrix.at(newRow).at(newCol).right = true;
        }
        if (dir == 3) { //goes up
            mazeMatrix.at(row).at(column).up = true;
            mazeMatrix.at(newRow).at(newCol).down = true;
        }

        cnt++;
        generateMaze(isVisited, newRow, newCol);
    }
}

bool Maze::findPath(std::vector< std::vector< bool > > &isVisited, int row, int column) {
    isVisited.at(row).at(column) = true;

    for (int dir = 0; dir < 4; dir++) {
        int newRow = row + dy.at(dir);
        int newCol = column + dx.at(dir);

        if (newRow >= mazeMatrix.size() || newRow < 0 || newCol >= mazeMatrix.at(0).size() || newCol < 0) continue;
        if (isVisited.at(newRow).at(newCol)) continue;

        if (dir == 0 && !mazeMatrix.at(row).at(column).right) continue;
        if (dir == 1 && !mazeMatrix.at(row).at(column).down) continue;
        if (dir == 2 && !mazeMatrix.at(row).at(column).left) continue;
        if (dir == 3 && !mazeMatrix.at(row).at(column).up) continue;

        if (findPath(isVisited, newRow, newCol)) {
            mazeMatrix.at(row).at(column).isPath = true;
            return true;
        }
    }

    if (row == mazeMatrix.size() - 1 && column == mazeMatrix.at(0).size() - 1) {
        mazeMatrix.at(row).at(column).isPath = true;
        return true;
    }
    else return false;
}

void Maze::printMaze() {
    int rowCells = 0, columnCells = -1; 
    for (int i = 0; i < numRows*2+1; i++){
        columnCells = -1;

        for (int j = 0; j < numColumns*4+1; j++) {
            if (i % 2 == 0) {
                if(j % 4 == 0) {
                    std::cout << '+';
                    columnCells++;
                }
                else {
                    if (rowCells >= numRows || !mazeMatrix.at(rowCells).at(columnCells).up) std::cout << '-';
                    else std::cout << ' ';
                }                
            }
            else {
                if (j % 4 == 0) {
                    columnCells++;
                    if (columnCells >= numColumns || !mazeMatrix.at(rowCells).at(columnCells).left) std::cout << '|';
                    else std::cout << ' ';
                }
                else if (j % 4 == 2 && mazeMatrix.at(rowCells).at(columnCells).isPath) {
                    std::cout << '.';
                }
                else std::cout << ' ';
            }
        }

        if (i % 2 == 1) rowCells++;
        std::cout << std::endl;
    }

}


int main(int argc, char* argv[]) {
    int numRows, numColumns, seed;
    numRows = atoi(argv[1]);
    numColumns = atoi(argv[2]);

    if (argc > 3) {
        seed = atoi(argv[3]);
    }
    else {
        seed = time(0);
    }
    srand(seed);
    
    Maze maze(numRows, numColumns);
    maze.printMaze();

    return 0;
}
