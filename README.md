/*In this question, you are given a binary string of length T. Now you need to create two permutations of this string: S1 and S2 such that the ‘longest common subsequence’ between the two newly created strings is smallest.

Ex: Given string: 101, you can find S1: 110 and S2: 011, The longest common subsequence between S1 and S2 is 11 and the length is 2. There cannot be any two permutations of the given string where the LCS is less than 2
Ex: Given 0111, then S1 should be: 1101, and S2: 0111, the smallest LCS will be 2 char long.*/

#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;

int shorty = 2;		//Initialization
vector<string> v1;	//Vector initialization

int perm(string x) {		//Function to find the permutations
    int bread = x.length();
    sort(x.begin(), x.end());
    while(1) {			//While Begin
        v1.push_back(x);
        int y = bread - 1;
        while(x[y-1] >= x[y]){	//While Begin
            if(--y == 0) {
                return 0;
            }	//While End
        }	//While End
        int z = bread - 1;
        while(z > y && x[z] <= x[y-1]) {	//While Begin
            z--;
        }	//While End
        swap(x[y-1],x[z]);
        reverse(x.begin()+y, x.end());
    }
    
}

void least(char *S1, char *S2, int a, int b) {		//Function to find the LCS
    int leastrix[a + 1][b + 1];		//Creation of the Matrix
    for (int k = 0; k <= a; k++) {
        for (int l = 0; l <= b; l++) {
            if (k == 0 || l == 0) {
                leastrix[k][l] = 0;
            }
            else if (S1[k - 1] == S2[l - 1]) {
                leastrix[k][l] = leastrix[k - 1][l - 1] + 1;
            }
            else {
                leastrix[k][l] = max(leastrix[k - 1][l], leastrix[k][l - 1]);
            }
        }
    }
        int indice = leastrix[a][b];
        char lcs[indice + 1];
        lcs[indice] = '\0';
        int  k = a, l = b;
        while (k > 0 && l > 0) {		//While Begins
            if (S1[k - 1] == S2[l - 1]) {
                lcs[indice - 1] = S1[l - 1];
                k--;
                l--;
                indice--;
            }
            else if (leastrix[k - 1][l] > leastrix[k][l - 1]) {
                k--;
            }
            else {
                l--;
            }
        }	//While End
        if(strlen(lcs) > shorty) {
            shorty = strlen(lcs);
        }
        if(strlen(S1) > 2 && strlen(S2) > 2) {
            cout << "S1 : " << S1 << "\nS2 : " << S2 << "\nLCS: " << lcs << endl;
            cout << "Shortest pair of permutations : " << shorty << endl;
        }    
}//End of Function


//Driver Code
int main()
{
    string q;
    cout << "Enter string" << endl;
    cin >> q;
    int loung = q.length();
    if(q.length() == 1 || q.length() == 0) {	//Validation of the String
        cout << "Please Check the string" << endl;
        return 0;
    }
    if(q.length() == 2) {
        cout << q << endl;
        cout << "Only two characters so No Result" << endl;
        return 0;
    }
    cout << "For this string, the permutations are:" << endl;
    perm(q);		//Calling the Function for Permutations
    for(int i=0;i<v1.size();i++) {
        cout << v1[i] << endl;
    }
    cout << "For each pair of possible permutations:" << endl;
    string aa,aaa;	//Variable Declaration
    for(int i=0;i<v1.size();i++) {
        if(i+1 < v1.size()) {
        aa = v1[i];
        aaa = v1[i+1];
        char c[aa.length()+1], d[aaa.length()+1];
        strcpy(c, aa.c_str());
        strcpy(d,aaa.c_str());
        least(c,d,strlen(c),strlen(d));	//Calling the Functions for LCS
        }
        
    }
    cout << "Shortest LCS will be: " << shorty << endl;
    return 0;
}//End of Main
