#include <iostream>
#include <string.h>
#include <vector>

#define pii pair<int, int>

using namespace std;

const int length = 555;  //행의 개수
const int MAX_TEAM = 50;  //팀의 개수
const int MAX_SEASON = 17;  //시즌의 개수. 2006년 이후부터

struct ResultGame {
    pii SetScore;  //총 세트 스코어
    vector<pii> SetGameScore;  //각 세트당 스코어
    bool WinLose;  //지면 0, 이기면 1
};

struct Team_data {
    char TeamName[50];  //팀의 이름
    
    vector<ResultGame*> R_Game;
    
    int NumGame;  //총 경기 수
    int NumWinGame, NumLoseGame;  // 총 이긴 경기 수, 진 경기 수
    int NumWinSetGame, NumLoseSetGame;  //총 이긴 세트 수, 진 세트 수
    int MyScore, OpponentScore; //총 이긴 점수, 총 진 점수
    
    Team_data(){};
    Team_data(int g, int wg, int lg, int wst, int lst, int ms, int os) : NumGame(g), NumWinGame(wg), NumLoseGame(lg), NumWinSetGame(wst), NumLoseSetGame(lst), MyScore(ms), OpponentScore(os) {};
};

string s[length+1];  //받아들이는 데이터
string tmp;

vector<Team_data*> Data;

//문자를 숫자로 변환
int MakeNum(char a) {
    return a - '0';
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0); cout.tie(0);
    
    //문자열 입력
    for(int i=0; i<length; i++)
        getline(cin, s[i]);
    

    /*
    //문자열 ASCII 코드
    for(int i=0; i<length; i++,  cout << '\n')
        for(int j=0; s[i][j]; j++)
            cout << (int)s[i][j] << ' ';
    */
    
    for(int i=0; i<length; i++) 
    {   
        Team_data* Next;
        
        char Name[50];
        
        //경기에서 상대한 팀의 이름을 추출해 Name에 저장하기
        int j;
        for(j=0; s[i][j]!=9; j++)
            Name[j] = s[i][j];
        Name[j] = '\0';
        j++;
        
        //지금까지 나온 팀이 아니라면 새로운 Team_data만들기
        int flag=0;
        for(auto Team : Data) {
            //두 문자열이 같다면
            if(!strcmp(Name, Team->TeamName)) {
                Next = Team;
                flag = 1;
                break;
            }
        }
        //팀을 새로 추가해야하는 경우
        if(!flag) {
            auto itTeam_data = new Team_data(0, 0, 0, 0, 0, 0, 0);
            strcpy(itTeam_data->TeamName, Name);
            Next = itTeam_data;
            Data.push_back(Next);
        }
        
        //seta:setb, score[i][0]-score[i][1], 왼쪽이 이겼는가 오른쪽이 이겼는가?
        int seta, setb, score[5][2];
        bool isWin;
        int lcnt = 0, rcnt = 0;
        
        seta = MakeNum(s[i][j]), setb = MakeNum(s[i][j+2])*10+MakeNum(s[i][j+3]); j+=5;
        for(int k=0; k<5; k++) {
            int isZero = MakeNum(s[i][j]);
            if(!isZero) {
                score[k][0] = score[k][1] = 0;
                j+=4;
            }
            else {
                score[k][0] = MakeNum(s[i][j])*10 + MakeNum(s[i][j+1]);
                score[k][1] = MakeNum(s[i][j+3])*10 + MakeNum(s[i][j+4]);
                if(score[k][0] > score[k][1])
                    lcnt++;
                else
                    rcnt++;
                j+=6;
            }
        }
        
        //이겼는가 졌는가?
        if(s[i][j] == -20)
            //이겼다
            isWin = true;
        else
            //졌다
            isWin = false;
        
        //데이터에 추가해주기
        auto Game = new ResultGame;
        Game->SetScore = {seta, setb};
        for(int k=0; k<5; k++) {
            if(isWin)
                if(lcnt < rcnt)
                    swap(score[k][0], score[k][1]);
            else
                if(lcnt > rcnt)
                    swap(score[k][0], score[k][1]);
            Game->SetGameScore.push_back({score[k][0], score[k][1]});
        }
        Game->WinLose = isWin;
        Next->R_Game.push_back(Game);
        
        Next->NumGame++;
        Next->NumWinGame += isWin; Next->NumLoseGame += !isWin;
        if(isWin && lcnt < rcnt)
            swap(lcnt, rcnt);
        if(!isWin && lcnt > rcnt)
            swap(lcnt, rcnt);
            
        Next->NumWinSetGame += lcnt;
        Next->NumLoseSetGame += rcnt;
        int tmp1=0, tmp2=0;
        bool isMinus = false;
        for(int k=0; k<5; k++) {
            tmp1 += (Next->R_Game.back())->SetGameScore[k].first;
            tmp2 += (Next->R_Game.back())->SetGameScore[k].second;
            if((Next->R_Game.back())->SetGameScore[k].first < 0 || (Next->R_Game.back())->SetGameScore[k].second < 0)
                isMinus = true;
        }
        if(!isMinus) {
            Next->MyScore += tmp2;
            Next->OpponentScore += tmp1;
        }
    }
    
    for(auto x : Data) {
        cout << "Team Name: " << x->TeamName << '\n';
        cout << "Total number of matches: " << x->NumGame << '\n';
        cout << "Total number of win, lose matches: " << x->NumWinGame << ' ' << x->NumLoseGame << '\n';
        cout << "Total number of win, lose set matches: " << x->NumWinSetGame << ' ' << x->NumLoseSetGame << '\n';
        cout << "My score and Opponent score: " << x->MyScore << ' ' << x->OpponentScore << '\n';
        cout << '\n';
    }
    
    return 0;
}
