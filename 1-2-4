#include "stdafx.h"
#include <string>
#include <vector>
#include <fstream>
#include <tuple>
#include <algorithm>
#include <cstdlib>
#include <iostream>
#include <cmath>
using namespace std;

char filler[100];
string str;
int temp;
bool compareTuples(const tuple<int, int, int> &first, const tuple<int, int, int> &second);

class DSU
{
public:
	vector<int> parent, rank;
	bool victory = true;
	DSU(int N)
	{
		for (int i = 0; i < N; i++)
		{
			parent.push_back(i + 1);
			rank.push_back(0);
		}
	}
	int find(int x)
	{
		if (x == 0) return -1;
		return (x == parent[x - 1] ? x : parent[x - 1] = find(parent[x - 1]));
	}

	void unite(int x, int y)
	{
		if ((x = find(x)) == (y = find(y)))
		{
			victory = false;
			return;
		}

		if (rank[x - 1] <  rank[y - 1])
			parent[x - 1] = y;
		else
			parent[y - 1] = x;

		if (rank[x - 1] == rank[y - 1])
			++rank[x - 1];
	}
};

class Graph
{
	char type;
	int peaks, edges;
	bool o, w;
	vector<vector<int>> adjmatr; //c
	vector<vector<pair<int, int>>> adjlist; //l
	vector<vector<tuple<int, int, int>>> wadjlist;
	vector<pair<int, int>> edgelist;//e
	vector<tuple<int, int, int>> wedgelist;
	
public:
	Graph(int N = 1)
	{
		peaks = N;
	}
	~Graph()
	{
	}
	void readGraph(string fileName)
	{
		ifstream input(fileName);
		input >> filler;
		switch (filler[0])
		{
		case 'C':
		{
			type = 'C';
			getline(input, str);
			peaks = str[1] - '0';
			for (int j = 2; j < str.length(); j++)
				peaks = peaks * 10 + (str[j] - '0');
			break;
		}
		case 'L':
		{
			type = 'L';
			getline(input, str);
			peaks = str[1] - '0';
			for (int j = 2; j < str.length(); j++)
				peaks = peaks * 10 + (str[j] - '0');
			break;
		}
		case 'E':
		{
			type = 'E';
			getline(input, str);
			peaks = str[1] - '0';
			int i = 2;
			while (str[i] != ' ')
			{
				peaks = peaks * 10 + (str[i] - '0');
				i++;
			}
			i++;
			edges = str[i] - '0';
			i++;
			while (i < str.length())
			{
				edges = edges * 10 + (str[i] - '0');
				i++;
			}
			break;
		}
		}
		input >> filler;
		if (filler[0] == '0') o = false; else o = true;
		input >> filler;
		if (filler[0] == '0') w = false; else w = true;

		if (type == 'C')
		{
			getline(input, str);
			int linepos = 0;
			while (getline(input, str))
			{
				adjmatr.push_back(vector<int>());
				temp = str[0] - '0';
				for (int i = 1; i < str.length(); i++)
					if (str[i] != ' ')
					{
						temp *= 10; temp += str[i] - '0';
					}
					else
					{
						adjmatr[linepos].push_back(temp);
						temp = str[i + 1] - '0';
						i++;
					}
				adjmatr[linepos].push_back(temp);
				linepos++;
			}
		}
		if (type == 'L')
		{
			if (!w)
			{
				getline(input, str);
				int linepos = 0;
				while (getline(input, str))
				{
					adjlist.push_back(vector<pair<int, int>>());
					temp = str[0] - '0';
					for (int i = 1; i < str.length(); i++)
						if (str[i] != ' ')
						{
							temp *= 10; temp += str[i] - '0';
						}
						else
						{
							adjlist[linepos].push_back(make_pair(linepos + 1, temp));
							temp = str[i + 1] - '0';
							i++;
						}
					adjlist[linepos].push_back(make_pair(linepos + 1, temp));
					linepos++;
				}
			}
			else
			{
				int tempTwo;
				getline(input, str);
				int linepos = 0;
				while (getline(input, str))
				{
					wadjlist.push_back(vector<tuple<int, int, int>>());
					temp = str[0] - '0';
					for (int i = 1; i < str.length(); i++)
						if (str[i] != ' ')
						{
							if (str[i] != ',')
							{
								temp *= 10;
								temp += str[i] - '0';
							}
							else
							{
								tempTwo = temp;
								temp = str[i + 1] - '0';
								i++;
								continue;
							}
						}
						else
						{
							wadjlist[linepos].push_back(make_tuple(linepos + 1, tempTwo, temp));
							temp = str[i + 1] - '0';
							i++;
						}
					wadjlist[linepos].push_back(make_tuple(linepos + 1, tempTwo, temp));
					linepos++;
				}
			}
		}

		if (type == 'E')
		{
			if (!w)
			{
				getline(input, str);
				int linepos = 0;
				while (getline(input, str))
				{
					edgelist.push_back(pair<int, int>());
					edgelist[linepos].first = str[0] - '0';
					int i = 1;
					while (str[i] != ' ')
					{
						edgelist[linepos].first = edgelist[linepos].first * 10 + str[i] - '0';
						i++;
					}
					i++;
					edgelist[linepos].second = str[i] - '0';
					i++;
					while (i < str.length())
					{
						edgelist[linepos].second = edgelist[linepos].second * 10 + str[i] - '0';
						i++;
					}
					linepos++;
				}
			}
			else
			{
				getline(input, str);
				int linepos = 0;
				while (getline(input, str))
				{
					wedgelist.push_back(tuple<int, int, int>());
					get<0>(wedgelist[linepos]) = str[0] - '0';
					int i = 1;
					while (str[i] != ' ')
					{
						get<0>(wedgelist[linepos]) = get<0>(wedgelist[linepos]) * 10 + str[i] - '0';
						i++;
					}
					i++;
					get<1>(wedgelist[linepos]) = str[i] - '0';
					i++;
					while (str[i] != ' ')
					{
						get<1>(wedgelist[linepos]) = get<1>(wedgelist[linepos]) * 10 + str[i] - '0';
						i++;
					}
					i++;
					get<2>(wedgelist[linepos]) = str[i] - '0';
					i++;
					while (i < str.length())
					{
						get<2>(wedgelist[linepos]) = get<2>(wedgelist[linepos]) * 10 + str[i] - '0';
						i++;
					}
					linepos++;
				}
			}
		}
	}
	void writeGraph(string fileName)
	{
		ofstream output(fileName);
		if (type == 'C')
		{
			output << "C" << ' ' << peaks << endl;
			o ? output << 1 : output << 0;
			output << ' ';
			w ? output << 1 : output << 0;
			output << endl;
			for (int i = 0; i < adjmatr.size(); i++)
			{
				output << adjmatr[i][0];
				for (int j = 1; j < adjmatr[i].size(); j++)
					output << ' ' << adjmatr[i][j];
				output << endl;
			}
		}
		if (type == 'L')
		{
			output << "L" << ' ' << peaks << endl;
			o ? output << 1 : output << 0;
			output << ' ';
			w ? output << 1 : output << 0;
			output << endl;
			if (!w)
			{
				for (int i = 0; i < adjlist.size(); i++)
				{
					if (adjlist[i].size() != 0) output << adjlist[i][0].second;
					for (int j = 1; j < adjlist[i].size(); j++)
						output << ' ' << adjlist[i][j].second;
					output << endl;
				}
			}
			else
			{
				for (int i = 0; i < wadjlist.size(); i++)
				{
					if (wadjlist[i].size() != 0) output << get<1>(wadjlist[i][0]) << ',' << get<2>(wadjlist[i][0]);
					for (int j = 1; j < wadjlist[i].size(); j++)
						output << ' ' << get<1>(wadjlist[i][j]) << ',' << get<2>(wadjlist[i][j]);
					output << endl;
				}
			}
		}
		if (type == 'E')
		{
			output << "E" << ' ' << peaks << ' ' << edges << endl;
			o ? output << 1 : output << 0;
			output << ' ';
			w ? output << 1 : output << 0;
			output << endl;
			if (!w)
				for (int i = 0; i < edgelist.size(); i++)
					output << edgelist[i].first << ' ' << edgelist[i].second << endl;
			else
				for (int i = 0; i < wedgelist.size(); i++)
					if (get<2>(wedgelist[i]) != -1)
						output << get<0>(wedgelist[i]) << ' ' << get<1>(wedgelist[i]) << ' ' << get<2>(wedgelist[i]) << endl;
		}
	}
	void addEdge(int from, int to, int weight = 1)
	{
		if (type == 'C')
			adjmatr[from - 1][to - 1] = weight;
		if (type == 'L')
		{
			if (!w)
			{
				adjlist[from - 1].push_back(make_pair(from, to));
				if (!o)
					adjlist[to - 1].push_back(make_pair(to, from));
			}
			else
			{
				wadjlist[from - 1].push_back(make_tuple(from, to, weight));
				if (!o)
					wadjlist[to - 1].push_back(make_tuple(to, from, weight));
			}
		}
		if (type == 'E')
		{
			if (!w)
				edgelist.push_back(make_pair(from, to));
			else
				wedgelist.push_back(tuple<int, int, int>(from, to, weight));
		}
	}
	void removeEdge(int from, int to)
	{
		if (type == 'C')
			adjmatr[from - 1][to - 1] = 0;
		if (type == 'L')
		{
			if (!w)
			{
				for (int i = 0; i < adjlist[from - 1].size(); i++)
					if (adjlist[from - 1][i].second == to) adjlist[from - 1].erase(adjlist[from - 1].begin() + i);
				if (!o)
					for (int i = 0; i < adjlist[to - 1].size(); i++)
						if (adjlist[to - 1][i].second == from) adjlist[to - 1].erase(adjlist[to - 1].begin() + i);
			}
			else
			{
				for (int i = 0; i < wadjlist[from - 1].size(); i++)
					if (get<1>(wadjlist[from - 1][i]) == to) wadjlist[from - 1].erase(wadjlist[from - 1].begin() + i);
				if (!o)
					for (int i = 0; i < wadjlist[to - 1].size(); i++)
						if (get<1>(wadjlist[to - 1][i]) == from) wadjlist[to - 1].erase(wadjlist[to - 1].begin() + i);
			}
		}
		if (type == 'E')
		{
			if (!w)
			{
				for (int i = 0; i < edgelist.size(); i++)
					if (edgelist[i].first == from && edgelist[i].second == to)
						edgelist.erase(edgelist.begin() + i);
			}
			else
			{
				for (int i = 0; i < wedgelist.size(); i++)
					if (get<0>(wedgelist[i]) == from && get<1>(wedgelist[i]) == to)
						wedgelist.erase(wedgelist.begin() + i);
			}
		}
	}
	int changeEdge(int from, int to, int newWeight)
	{
		int oldWeight;
		if (type == 'C')
		{
			oldWeight = adjmatr[from - 1][to - 1];
			adjmatr[from - 1][to - 1] = newWeight;
		}
		if (type == 'L')
		{
			for (int i = 0; i < wadjlist[from - 1].size(); i++)
				if (get<1>(wadjlist[from - 1][i]) == to)
				{
					oldWeight = get<2>(wadjlist[from - 1][i]);
					get<2>(wadjlist[from - 1][i]) = newWeight;
				}
			if (!o)
			{
				for (int i = 0; i < wadjlist[to - 1].size(); i++)
					if (get<1>(wadjlist[to - 1][i]) == from)
					{
						oldWeight = get<2>(wadjlist[to - 1][i]);
						get<2>(wadjlist[to - 1][i]) = newWeight;
					}
			}
		}
		if (type == 'E')
			for (int i = 0; i < wedgelist.size(); i++)
				if (get<0>(wedgelist[i]) == from && get<1>(wedgelist[i]) == to)
				{
					oldWeight = get<2>(wedgelist[i]);
					get<2>(wedgelist[i]) = newWeight;
				}
		return oldWeight;
	}
	void transformToAdjList()
	{
		if (type == 'C')
		{
			if (!w)
				for (int i = 0; i < adjmatr.size(); i++)
				{
					adjlist.push_back(vector<pair<int, int>>());
					for (int j = 0; j < adjmatr[i].size(); j++)
						if (adjmatr[i][j] != 0)
							adjlist[i].push_back(make_pair(i + 1, j + 1));
				}
			else
				for (int i = 0; i < adjmatr.size(); i++)
				{
					wadjlist.push_back(vector<tuple<int, int, int>>());
					for (int j = 0; j < adjmatr[i].size(); j++)
						if (adjmatr[i][j] != 0)
							wadjlist[i].push_back(tuple<int, int, int>(i + 1, j + 1, adjmatr[i][j]));
				}
			adjmatr = vector<vector<int>>();
		}
		if (type == 'E')
		{
			if (!w)
			{
				for (int i = 0; i < peaks; i++)
					adjlist.push_back(vector<pair<int, int>>()); 
				for (int i = 0; i < peaks; i++)
				{
					for (int j = 0; j < edgelist.size(); j++)
						if (edgelist[j].first == i + 1)
						{
							adjlist[i].push_back(make_pair(i + 1, edgelist[j].second));
							if (!o)
								adjlist[edgelist[j].second - 1].push_back(make_pair(edgelist[j].second, i + 1));
						}
				}
			}
			else
			{
				for (int i = 0; i < peaks; i++)
					wadjlist.push_back(vector<tuple<int, int, int>>());
				for (int i = 0; i < peaks; i++)
				{
					for (int j = 0; j < wedgelist.size(); j++)
						if (get<0>(wedgelist[j]) == i + 1)
						{
							wadjlist[i].push_back(tuple<int, int, int>(get<0>(wedgelist[j]), get<1>(wedgelist[j]), get<2>(wedgelist[j])));
							if (!o)
								wadjlist[get<1>(wedgelist[j]) - 1].push_back(tuple<int, int, int>(get<1>(wedgelist[j]), get<0>(wedgelist[j]), get<2>(wedgelist[j])));
						}
				}
			}
			edgelist = vector<pair<int, int>>();
			wedgelist = vector<tuple<int, int, int>>();
		}
		type = 'L';
	}
	void transformToListOfEdges()
	{
		if (type == 'C')
		{
			if (!w)
				for (int i = 0; i < adjmatr.size(); i++)
				{
					edgelist.push_back(pair<int, int>());
					for (int j = 0; j < adjmatr[i].size(); j++)
						if (adjmatr[i][j] != 0)
							edgelist.push_back(make_pair(i + 1, j + 1));
				}
			else
				for (int i = 0; i < adjmatr.size(); i++)
				{
					for (int j = 0; j < adjmatr[i].size(); j++)
						if (adjmatr[i][j] != 0)
							wedgelist.push_back(tuple<int, int, int>(i + 1, j + 1, adjmatr[i][j]));
				}
			adjmatr = vector<vector<int>>();
		}
		if (type == 'L')
		{
			if (!w)
			{
				for (int i = 0; i < adjlist.size(); i++)
					for (int j = 0; j < adjlist[i].size(); j++)
					{
						bool next = false;
						if (!o && adjlist[i][j].first < adjlist[i][j].second)
							for (int k = 0; k < adjlist[adjlist[i][j].second - 1].size(); k++)
								if (adjlist[adjlist[i][j].second - 1][k].second == adjlist[i][j].first) next = true;
						if (next) continue;
						else edgelist.push_back(make_pair(adjlist[i][j].first, adjlist[i][j].second));
					}
			}
			else
			{
				for (int i = 0; i < wadjlist.size(); i++)
					for (int j = 0; j < wadjlist[i].size(); j++)
					{
						bool next = false;
						if (!o && get<0>(wadjlist[i][j]) < get<1>(wadjlist[i][j]))
							for (int k = 0; k < wadjlist[get<1>(wadjlist[i][j]) - 1].size(); k++)
								if (get<1>(wadjlist[get<1>(adjlist[i][j])][k]) == get<0>(wadjlist[i][j])) next = true;
						if (next) continue;
						else wedgelist.push_back(tuple<int, int, int>(get<0>(wadjlist[i][j]), get<1>(wadjlist[i][j]), get<2>(wadjlist[i][j])));
					}
			}
			adjlist = vector<vector<pair<int, int>>>();
			wadjlist = vector<vector<tuple<int, int, int>>>();
		}
		type = 'E';
	}
	void transformToAdjMatrix()
	{
		if (type == 'L')
		{
			if (!w)
			{
				for (int i = 0; i < peaks; i++)
				{
					adjmatr.push_back(vector<int>());
					for (int j = 0; j < adjlist.size(); j++)
						adjmatr[i].push_back(0);
				}
				for (int i = 0; i < adjlist.size(); i++)
					for (int j = 0; j < adjlist[i].size(); j++)
						adjmatr[i][adjlist[i][j].second - 1] = 1;
			}
			else
			{
				for (int i = 0; i < wadjlist.size(); i++)
				{
					adjmatr.push_back(vector<int>());
					for (int j = 0; j < wadjlist.size(); j++)
						adjmatr[i].push_back(0);
				}
				for (int i = 0; i < wadjlist.size(); i++)
					for (int j = 0; j < wadjlist[i].size(); j++)
						adjmatr[i][get<1>(wadjlist[i][j]) - 1] = get<2>(wadjlist[i][j]);
			}
			adjlist = vector<vector<pair<int, int>>>();
			wadjlist = vector<vector<tuple<int, int, int>>>();
		}
		if (type == 'E')
		{
			if (!w)
			{
				for (int i = 0; i < peaks; i++)
				{
					adjmatr.push_back(vector<int>());
					for (int j = 0; j < peaks; j++)
						adjmatr[i].push_back(0);
				}
				for (int i = 0; i < edgelist.size(); i++)
				{
					adjmatr[edgelist[i].first - 1][edgelist[i].second - 1] = 1;
					if (!o)
						adjmatr[edgelist[i].second - 1][edgelist[i].first - 1] = 1;
				}
			}
			else
			{
				for (int i = 0; i < peaks; i++)
				{
					adjmatr.push_back(vector<int>());
					for (int j = 0; j < peaks; j++)
						adjmatr[i].push_back(0);
				}
				for (int i = 0; i < wedgelist.size(); i++)
				{
					adjmatr[get<0>(wedgelist[i]) - 1]
						[get<1>(wedgelist[i]) - 1] =
						get<2>(wedgelist[i]);
					if (!o)
						adjmatr[get<1>(wedgelist[i]) - 1][get<0>(wedgelist[i]) - 1] = get<2>(wedgelist[i]);
				}
			}
			edgelist = vector<pair<int, int>>();
			wedgelist = vector<tuple<int, int, int>>();
		}
		type = 'C';
	}
	Graph getSpaingTreePrima()
	{
		Graph g(peaks);
		vector<vector<int>> resmatr;
		vector<vector<tuple<int, int, int>>> wedgelistsample;
		vector<int> line;
		int min = -1, minInLine;
		int minIndx, idx = -1, from = 0;
		return getSpaingTreeBoruvka();
		g.w = true;
		g.o = false;
		g.type = 'C';
		line.push_back(1);
		while (line.size() < g.peaks)
		{
			idx = -1;
			min = -1;
			for (int item = 0; item<line.size(); item++)
			{
				minInLine = -1;
				minIndx = 0;
				for (int i = 0; i < wadjlist[line[item] - 1].size(); i++)
				{
					if (get<2>(wadjlist[line[item] - 1][i]) < minInLine && get<2>(wadjlist[line[item] - 1][i]) != 0 && find(line.begin(), line.end(), get<1>(wadjlist[line[item] - 1][i])) == line.end())
					{
						minInLine = get<2>(wadjlist[line[item] - 1][i]);
						minIndx = get<1>(wadjlist[line[item] - 1][i]) - 1;
					}
				}
				if (minInLine < min && find(line.begin(), line.end(), minIndx + 1) == line.end())
				{
					min = minInLine;
					idx = minIndx;
					from = line[item] - 1;
				}
			}
			if (idx != -1)
			{
				for (int z = 0;z < wadjlist[from].size();z++)
					if (get<1>(wadjlist[from][z]) == idx)
						wedgelistsample[from].push_back(make_tuple(from + 1, idx + 1, get<2>(wadjlist[from][z])));
				line.push_back(idx + 1);
			}
		}
		g.wadjlist = wedgelistsample;
		g.edges = peaks - 1;

		return g;
	}
	Graph getSpaingTreeKruscal()
	{
		if (type != 'E') transformToListOfEdges();
		Graph g(peaks);
		g.w = true;
		g.o = false;
		g.type = 'E';
		DSU dsu(peaks);
		sort(wedgelist.begin(), wedgelist.end(), compareTuples);
		for (int i = 0; i < wedgelist.size(); i++)
		{
			if (dsu.find(get<0>(wedgelist[i])) != dsu.find(get<1>(wedgelist[i])))
			{
				dsu.unite(get<0>(wedgelist[i]), get<1>(wedgelist[i]));
				g.wedgelist.push_back(make_tuple(get<0>(wedgelist[i]), get<1>(wedgelist[i]), get<2>(wedgelist[i])));
			}
		}
		g.edges = peaks - 1;
		return g;
	}
	Graph getSpaingTreeBoruvka()
	{
		if (type != 'E') transformToAdjList();
		vector<tuple<int, int, int>> wedgelistsample;
		Graph g(peaks);
		g.w = true;
		g.o = false;
		g.type = 'E';
		DSU dsu(peaks);
		int peaksamount = peaks;
		for (int i = 0; i < peaks; i++)
			wedgelistsample.push_back(make_tuple(0, 0, 0));
		while (peaksamount > 1)
		{
			for (int i = 0; i < edges; i++)
			{
				int from = dsu.find(get<0>(wedgelist[i])), to = dsu.find(get<1>(wedgelist[i]));
				if (from == to)
					continue;
				else
				{
					if (get<2>(wedgelistsample[from - 1]) == 0 || get<2>(wedgelistsample[from - 1]) > get<2>(wedgelist[i]))
						wedgelistsample[from - 1] = wedgelist[i];
					if (get<2>(wedgelistsample[to - 1]) == 0 || get<2>(wedgelistsample[to - 1]) > get<2>(wedgelist[i]))
						wedgelistsample[to - 1] = wedgelist[i];
				}
			}
			for (int i = 0; i < wedgelistsample.size(); i++)
			{
				if (dsu.find(get<0>(wedgelistsample[i])) != dsu.find(get<1>(wedgelistsample[i])))
				{
					dsu.unite(get<0>(wedgelistsample[i]), get<1>(wedgelistsample[i]));
					g.wedgelist.push_back(make_tuple(get<0>(wedgelistsample[i]), get<1>(wedgelistsample[i]), get<2>(wedgelistsample[i])));
					peaksamount--;
				}
			}
			for (int z = 0; z < wedgelistsample.size(); z++)
				wedgelistsample[z] = make_tuple(0, 0, 0);
		}
		g.edges = peaks - 1;
		return g;
	}

	int checkBipart(vector<char> &marks)
	{
		if (type != 'L') transformToAdjList();
		vector<int> queue, visited;
		marks[0] = 'A';
		queue.push_back(1);
		visited.push_back(1);
		while (queue.size() != 0)
		{
			for (int i = 0; i < adjlist[queue.back() - 1].size(); i++)
			{
				if (find(visited.begin(), visited.end(), adjlist[queue.back() - 1][i].second) == visited.end() && find(queue.begin(), queue.end(), adjlist[queue.back() - 1][i].second) == queue.end())
					queue.insert(queue.begin(), adjlist[queue.back() - 1][i].second);
				if (marks[adjlist[queue.back() - 1][i].second - 1] == ' ')
					if (marks[queue.back() - 1] == 'A')
						marks[adjlist[queue.back() - 1][i].second - 1] = 'B';
					else
						marks[adjlist[queue.back() - 1][i].second - 1] = 'A';
				else
					if (marks[adjlist[queue.back() - 1][i].second - 1] == marks[queue.back() - 1])
						return 0;
			}
			visited.push_back(queue.back());
			queue.erase(queue.end() - 1, queue.end());
		}
		return 1;
	}

	bool FindChain(int vert, vector<pair<int, int>> &bipart, vector<int> &visited, vector<int> oldQueue, vector<char> &marks)
	{
		if (find(visited.begin(), visited.end(), vert) != visited.end()) return false; 
		visited.push_back(vert);
		if (marks[vert - 1] == 'B')
		{
			for (int i = 0; i < bipart.size(); i++)
				if (bipart[i].second == vert)
				{
					oldQueue = vector<int>();
					oldQueue.push_back(bipart[i].first);
					return FindChain(bipart[i].first, bipart, visited, oldQueue, marks);
				}
		}
		vector<int> newQueue;
		for (int i = 0; i < oldQueue.size(); i++)
			for (int j = 0; j < adjlist[oldQueue[i] - 1].size(); j++)
			{
				if (find(oldQueue.begin(), oldQueue.end(), adjlist[oldQueue[i] - 1][j].second) == oldQueue.end())
					newQueue.push_back(adjlist[oldQueue[i] - 1][j].second);
				if (find(visited.begin(), visited.end(), adjlist[oldQueue[i] - 1][j].second) == visited.end())
				{
					int to = adjlist[oldQueue[i] - 1][j].second;
					bool notVis = true;
					for (int k = 0; k < bipart.size(); k++)
						if ((bipart[k].second == to && marks[oldQueue[i] - 1] == 'A') || (bipart[k].first == to && marks[oldQueue[i] - 1] == 'B'))
							notVis = false;
					if (notVis)
					{
						if (marks[oldQueue[i] - 1] == 'B') return true;
						bool changed = false;
						for (int k = 0; k < bipart.size(); k++)
							if (bipart[k].first == vert)
							{
								changed = true;
								bipart[k].second = to;
								break;
							}
						if (!changed)
							for (int k = 0; k < bipart.size(); k++)
								if (bipart[k].first == -1)
								{
									bipart[k].first = vert;
									bipart[k].second = to;
									break;
								}
						return true;
					}
				}
			}
		for (int i = 0; i < oldQueue.size(); i++)
			for (int j = 0; j < adjlist[oldQueue[i] - 1].size(); j++)
			{
				if (find(visited.begin(), visited.end(), adjlist[oldQueue[i] - 1][j].second) == visited.end())
				{
					int to = adjlist[oldQueue[i] - 1][j].second;
					bool notVis = true;
					for (int k = 0; k < bipart.size(); k++)
						if ((bipart[k].second == to && marks[oldQueue[i] - 1] == 'A') || (bipart[k].first == to && marks[oldQueue[i] - 1] == 'B'))
							notVis = false;
					if (find(visited.begin(), visited.end(), to) != visited.end()) continue;
					if (FindChain(to, bipart, visited, newQueue, marks))
					{
						if (marks[oldQueue[i] - 1] == 'B') return true;
						bool changed = false;
						for (int k = 0; k < bipart.size(); k++)
							if (bipart[k].first == vert)
							{
								changed = true;
								bipart[k].second = to;
								break;
							}
						if (!changed)
							for (int k = 0; k < bipart.size(); k++)
								if (bipart[k].first == -1)
								{
									bipart[k].first = vert;
									bipart[k].second = to;
									break;
								}
						return true;
					}
				}
			}
		return false;
	}

	vector<pair<int, int>>getMaximumMatchingBipart()
	{
		vector<pair<int, int>> bipart;
		vector<char> marks;
		vector<int> queue, visited;
		for (int i = 0; i < peaks; i++)
			marks.push_back(' ');
		if (checkBipart(marks) == 0) return bipart;
		for (int i = 0; i < marks.size(); i++)
			if (marks[i] == 'A')
				bipart.push_back(pair<int, int>(-1, -1));
		queue.push_back(0);
		for (int i = 0; i < peaks; i++)
		{
			queue[0] = i + 1;
			if (marks[i] != 'A') continue;
			FindChain(i + 1, bipart, visited, queue, marks);
			visited = vector<int>();
		}
		for (int j = 0; j < 5; j++)
		for (int i = 0; i < bipart.size(); i++)
			if (bipart[i].first == -1)
				bipart.erase(bipart.begin() + i);
		return bipart;
	}
};

bool compareTuples(const tuple<int, int, int> &first, const tuple<int, int, int> &second)
{
	return get<2>(first) < get<2>(second);
}

int main(void)
{
	Graph g;
	g.readGraph("input.txt");
	//g.transformToAdjList();
	//Graph gg = g.getSpaingTreePrima();
	//gg.transformToListOfEdges();
	vector<pair<int, int>> v = g.getMaximumMatchingBipart();
	for (int i = 0; i < v.size(); i++)
		cout << v[i].first << ' ' << v[i].second << endl;
	//gg.writeGraph("output.txt");
	//g.writeGraph("output.txt");
}
