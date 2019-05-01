# TicTacToe
Tic Tac Toe game using C# 

#include<bits/stdc++.h>
#define lli long long int
#define pb push_back
#define mod 1000000007
#define INF 1e18
#define SIZE 100001

using namespace std;

vector<lli> adj[SIZE];//adjacency list
bool visited[SIZE];//used in dfs and bfs
lli dist[SIZE],discover[SIZE],finish[SIZE];//dist for bfs,intime,outtime for dfs
lli t = 1;//time used for the dfs
stack<lli> st;//stack used for toposort

void dfs(lli s)
{
   visited[s] = 1;//making it visited
   discover[s] = t++;//assigning and incrementing time
   lli i,v;
   for(i=0;i<adj[s].size();i++)
   {
       v = adj[s][i];
       if(!visited[v])//if vertex is not visited then visit
           dfs(v);
   }
   st.push(s);//push into stack for toposort
   finish[s] = t++;//assigning the outtime
}


int main()
{
   lli u,v,i,j,k,N=0;
   pair<lli,lli> current;
  
   ifstream iFile("input.txt");//reading the input file
   string str,num;
  
   //read until the end of file
   while(!iFile.eof())
   {
       getline(iFile,str);//read the current line
       stringstream ss;
       ss<<str; //convert the line to stream of strings
      
       ss>>num; //read the line num
       stringstream(num)>>u;
      
       while(!ss.eof())
       {
           ss>>num;
           if(stringstream(num)>>v)
           {
               adj[u].pb(v); // read the adjacent vertices
           }
       }
       N++; //calculating the number of vertices
       sort(adj[u].begin(),adj[u].end()); //sorting the adjaceny list in case it is not sorted
   }
  
   for(i=1;i<=N;i++)
       dist[i] = INF;//assigning INF value which is arbitrary, here i have taken 10^18 as INF
   memset(visited , 0 , sizeof visited);
  
  
   //bfs
   queue<lli> q;//queue for bfs
   q.push(1); //pushing the source that is 1
   dist[1] = 0; //assigning distance of source to be 0
   visited[1] = 1;//making it visited
  
   while(!q.empty())
   {
       u = q.front(); q.pop();//front value of queue
       for(i=0;i<adj[u].size();i++)
       {
           v = adj[u][i];//adjacent vertex
           if(!visited[v])//if not visited,update the dist and push into the queue
           {
               visited[v] = 1;
               dist[v] = dist[u]+1;
               q.push(v);
           }
       }
   }
  
   multiset< pair<lli,lli> > s;//a special data structure for min heap
   for(i=1;i<=N;i++)
       s.insert(make_pair(dist[i],i));//for sorted distance
  
   cout<<"bfs results:"<<endl;
  
   while(!s.empty())
   {
       current = *s.begin(); s.erase(s.begin());
       i = current.second; j = current.first;
       if(j == INF)
       {
           cout<<i<<" INF"<<endl;
       }
       else
       {
           cout<<i<<" "<<j<<endl;
       }
   }  
  
   memset(visited , 0 , sizeof visited);
  
  
   //dfs
   for(i=1;i<=N;i++)
   {
       if(visited[i])//if visited continue else visit it by dfs
       {
           continue;
       }
       dfs(i);
   }
   for(i=1;i<=N;i++)
   {
       s.insert(make_pair(discover[i],i));//min heap for sorted discover time
   }
  
  
   cout<<"\n\ndfs results:"<<endl;
   while(!s.empty())
   {
       current = *s.begin(); s.erase(s.begin());
       i = current.second;
       cout<<i<<" "<<discover[i]<<" "<<finish[i]<<endl;
   }
  
   cout<<"\n\ntopological sort results:"<<endl;
  
   while(!st.empty())
   {
       i = st.top(); st.pop();
       cout<<i<<endl;
   }
  
}
